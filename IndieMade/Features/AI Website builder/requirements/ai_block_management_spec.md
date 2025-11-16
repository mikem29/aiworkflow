# AI Assistant Block Management Technical Specification

This document narrows the implementation requirements for the Drupal-based AI website builder so that HTML block creation, styling, and behavioral enhancements remain safe, reviewable, and consistent with IndieMade’s design system. It expands on the existing technical considerations by detailing block storage, CSS governance, JavaScript guardrails, and AI role separation.

## 1. Block Architecture and Lifecycle

- **Custom block type**: Introduce `ai_assistant_html_block`, a lightweight custom block type storing sanitized HTML, metadata, and provenance (AI role, originating prompt, timestamp).
- **Entity ownership**: Assign blocks to an `ai_assistant` Drupal user role. Editors retain publish rights; AI services can only create drafts, update revisions, or unpublish blocks it owns.
- **Permissions matrix**:
  - `ai_assistant` role: `create ai_assistant_html_block`, `edit own`, `view own`, `unpublish own`; explicitly deny `delete` to force human-controlled cleanup.
  - Human roles inherit `view`, `edit`, `publish`, `delete` according to existing workflow configuration.
- **Revision strategy**: Enable revision tracking and moderation states (Draft → Needs Review → Published). Only persist a revision when an editor approves an AI-proposed change or explicitly requests a checkpoint so the history reflects meaningful milestones. Diff viewers must highlight HTML changes per revision.
- **Revision retention**: Maintain a bounded set of the most recent approved AI revisions (e.g., last 5–10) flagged for fast lookup so the assistant can surface “last known good” options without trawling the full revision table.
- **Block registry**: Maintain a lookup table mapping page nodes to the AI-generated block UUIDs so the assistant can safely reuse or update blocks without scanning unrelated content.
- **Sharing across pages**: Support cloning by copying block entities while preserving provenance metadata; log cross-page usage for audit trails.
- **Undo & rollback**: Expose a conversational “Revert to prior AI revision” flow where the assistant surfaces the stored checkpoints, confirms the target with the editor, and issues the Drupal revision revert on the custom `ai_assistant_html_block` entity once approved. Note that complementary CSS rollback controls already exist in the stylesheet management requirements—rolling back HTML must coordinate with those styles when both changed together.

## 2. HTML Generation Guardrails

- **Schema-constrained prompts**: Supply the LLM with the allowed HTML subset (semantic elements, permitted attributes, class naming conventions) sourced from a Drupal-configured manifest.
- **Server-side sanitization**: Run generated markup through `xss_admin` filters plus a custom allowlist validator that checks for:
  - Disallowed inline scripts or event handlers.
  - External resource loading outside approved domains/CDNs.
  - Inline styles not registered in the style manifest.
- **Accessibility linting**: Automatically queue Axe-core (or equivalent) checks before promoting a block above “Draft” state; report violations back to the editor.

## 3. CSS Strategy

- **Theme manifest**: Generate and maintain a JSON manifest of design tokens (colors, typography, spacing), component class names, and responsive breakpoints. Expose via internal API and cache results for the assistant session.
- **Scoped utility classes**:
  - Prefer existing theme utility classes. Prompt templates instruct the AI to compose markup using these tokens instead of inventing new class names.
  - If new styling is unavoidable, require the AI to request a named utility (`aiu-hero-gradient`) via a “style request” endpoint rather than embedding raw CSS.
- **AI style library**:
  - Approved style requests materialize as SCSS partials under `themes/custom/<theme>/ai_assistant/`.
  - A build step compiles them into an `ai-assistant.css` library attached only to blocks referencing those utilities, preventing global bleed.
  - All additions pass through linting (stylelint) and a design-lead approval workflow before deployment.
- **Inline style fallback**: Disallow inline CSS except for controlled, whitelisted attributes (e.g., background-image URLs). Validation rejects any block that bypasses the library system.
- **Versioning and rollback**: Style partials track git history and Drupal configuration exports to ensure traceability; rollback reverts both CSS and associated block revisions.

## 4. JavaScript and Interaction Controls

- **Behavior catalog**:
  - Curate an approved library list (e.g., Bootstrap collapse, Swiper, PhotoSwipe) with documented initialization patterns.
  - Publish a machine-readable capabilities file describing each behavior, required data attributes, and configuration bounds.
- **Attachment model**:
  - Blocks declare behaviors via metadata (`ai_behaviors: [photoswipe_gallery]`). The Drupal module attaches the corresponding library and initializes via a central controller.
  - Runtime initialization passes through a sandboxed dispatcher that validates options against a JSON Schema before execution.
- **Custom logic requests**:
  - AI cannot inject raw `<script>` elements. To propose new interactions, it submits a “behavior request” ticket that a developer reviews and implements as a new catalog entry.
  - Small parametrized variations use an allowlisted options map (`animation_duration`, `autoplay`) validated server-side.
- **Security guardrails**: Content Security Policy forbids inline script execution (`unsafe-inline`). Drupal’s `drupalSettings` only exposes whitelisted configuration derived from the behavior catalog.

## 5. AI Role Separation (Phase 2 Readiness)

- **Role taxonomy**:
  - `ai_assistant.builder`: Generates initial layout and copy blocks.
  - `ai_assistant.quality`: Runs accessibility/style compliance checks and suggests remediations.
  - `ai_assistant.reviewer`: Produces diffs and rationale for editor approval.
  - `ai_assistant.tester`: Executes smoke tests on interactive behaviors (e.g., lightbox open/close).
- **Task routing**: A workflow engine (state machine or job queue) assigns block revisions to specialized roles; each action logs the responsible agent for auditability.
- **Permission scoping**: Builder can create/update drafts only; Quality can mark issues; Reviewer can recommend publish but not execute; Tester interacts with behaviors in a staging environment.
- **Observability**: Capture per-role metrics (success rate, human overrides) to refine prompts and adjust guardrails.

## 6. Operational Oversight

- **Logging**: For every AI block mutation, log prompt, delta HTML, CSS/JS attachments, validation outcomes, and reviewer decisions in a dedicated Drupal log channel.
- **Rate limiting**: Cap block creation/edit frequency per session to prevent runaway generation.
- **Disaster recovery**: Nightly snapshot of AI-generated blocks and style manifests; documented procedure to disable AI libraries quickly if regressions surface.
- **Governance workflow**: Editors receive a consolidated summary (diff, accessibility report, style manifest compliance) before approving publish transitions.

## 7. Open Implementation Questions

- What UI affordances best convey when an AI block references shared styles or behaviors that may affect other pages?
- Should style and behavior requests create Jira (or similar) tickets automatically for design/dev review, or remain within Drupal’s moderation queue?
- How do we sunset unused AI-generated utilities or blocks while preserving audit trails for already published pages?

This specification should feed into the next iteration of the technical considerations document and guide implementation tasks for the Drupal module, AI prompt orchestration, and review workflows.
