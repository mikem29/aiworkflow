# Product Overview

## Vision

Deliver an AI-assisted experience inside Drupal 11 that lets non-technical editors assemble entire pages by orchestrating reusable content blocks that respect the site’s design system, accessibility standards, and publishing workflows. The assistant should combine uploaded assets with generated content and structured Drupal entities so that pages can be built, revised, and published without needing direct developer intervention.

## Goals

- Shorten the time required to design and publish a new page composed of Drupal blocks.
- Maintain visual and structural consistency with the active Drupal theme and component library.
- Allow editors to upload, organize, and reuse assets across multiple pages without duplicate management effort.
- Provide conversational guidance and generative assistance while keeping human approvers in control of every change.
- Capture enough context (brand voice, layout constraints, compliance rules) so that future AI refinements continue to improve.

## Success Metrics

- **Time-to-first-publish**: 50% reduction in average elapsed time from concept to live page.
- **Editor satisfaction**: ≥ 80% positive feedback in quarterly surveys covering ease of use and trust in AI suggestions.
- **Theme adherence**: ≥ 95% of published blocks pass automated checks for required classes/styles.
- **Reuse ratio**: ≥ 60% of newly published blocks originate from saved block templates or patterns suggested by the assistant.
- **Error rate**: < 5% of AI-generated blocks require rework to meet accessibility (WCAG 2.1 AA) or governance standards.

## Out of Scope

- Building a new Drupal theme or design system from scratch.
- Managing third-party content feeds or headless delivery outside Drupal.
- Training or hosting foundational LLM models; the project only orchestrates existing APIs.
- Replacing mandatory human approvals for high-risk content (legal, compliance, marketing sign-off).
- Analytics dashboards beyond the metrics required to monitor assistant effectiveness.

## Personas

- **Site Builder**: Configures Drupal structure, defines block types, and governs which AI capabilities are available. Needs assurance that generated entities conform to Drupal best practices.
- **Content Editor**: Primary user prompting the assistant, uploading assets, and arranging blocks on specific pages. Requires guardrails, previews, and the ability to undo changes easily.
- **Design Lead / Brand Manager**: Owns the theme and style guide. Provides the tokens, typography scales, and component rules that the AI must honor.
- **Compliance Reviewer**: Performs final checks for regulated content. Needs transparency into AI decisions, change history, and quick access to supporting documentation.
- **Technical Administrator**: Oversees integrations, API keys, and security. Ensures the assistant follows organizational policies for data retention and access control.

