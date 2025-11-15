# Functional Requirements

## Asset Intake and Management

- The assistant shall provide an “Asset Manager” panel where editors can upload images, documents, and structured references associated with a page request.
- Uploaded assets must be stored as Drupal Media entities with deduplicated file handling and metadata (title, alt text, tags, copyright).
- The system must support bulk upload (drag-and-drop or multi-select) with progress indicators and error handling for unsupported file types.
- Editors must be able to search, filter, and reuse assets across pages; AI suggestions should surface relevant existing assets before requesting new uploads.
- AI-generated alt text or summaries must be presented as suggestions that editors can accept or edit before saving the media entity.
- The asset manager shall launch as a modal Media Library experience so editors can add, browse, and attach images without leaving the assistant workspace.
- Editors must be able to review existing Drupal image styles, preview how a style affects an asset, and request new styles that route to site builders for approval.
- Inline editing tools (crop, rotate, focal point selection, simple color adjustments) must be available during upload and on subsequent edits, with changes tracked as Media revisions.
- The assistant should leverage a curated plugin stack—Focal Point, Image Widget Crop, Media Library Edit, Responsive Image, ImageAPI Optimize, and Image Effects—to satisfy style previews, responsive variants, optimization, and quick edits.
- Image metadata (alt text, focal subject, detected people/objects, licensing notes, campaign tags) must be captured in structured fields so conversational prompts can target assets precisely.
- AI metadata suggestions (captions, keywords, accessibility descriptions) must surface with confidence scores and require human confirmation before persistence.
- Phase delivery:
  - **Phase I** focuses on reliable upload, editing, metadata enrichment, and reuse of human-supplied assets.
  - **Phase II** introduces optional AI image generation (e.g., Google Gemini) plus advanced semantic search powered by stored embeddings.
- The assistant must expose conversational commands that map to predefined image style recipes (e.g., “apply the hero banner crop”) while preserving the original source asset for rollback.

## Page and Block Lifecycle

- Editors can start from a plain-language brief (e.g., “Create an About Us page highlighting our leadership team and mission”) that seeds the AI plan.
- The assistant must propose a structured page outline comprised of Drupal block placements, including block type, layout region, and purpose.
- Users can accept, reject, or rearrange proposed blocks before they are saved.
- For each block, the assistant should create draft content using the selected block type (custom block, paragraph, layout builder section, etc.) and populate fields with generated copy, assets, and links.
- Editors must have the ability to:
  - Reorder blocks via drag-and-drop that maps back to the underlying Layout Builder or Paragraph ordering.
  - Edit block content manually with inline form elements.
  - Save blocks to a shared library as reusable templates/patterns with tagging and preview imagery.
  - Delete blocks with confirmation to prevent accidental removals.
- Page drafts should support versioning so that editors can revert to a prior AI iteration.

## AI Interaction Flows

- The assistant must maintain conversational context, allowing editors to refine instructions (e.g., “Make the hero headline more energetic”).
- When proposing changes, the assistant should provide a diff-style summary that highlights what content or structure will change before the user commits it.
- The assistant must be able to query existing blocks, layouts, and assets to reference them in new suggestions (“Reuse the Testimonials block from the Home page”).
- Users can provide corrective feedback (“Do not use stock photos with people”) that is persisted for the current page session and optionally stored as organization-wide guidance.
- The assistant should offer optional mode toggles such as “Draft content only,” “Structure only,” or “Full page assembly” to accommodate varying workflows.

## Content Governance

- All AI-created content must be saved in draft status until a user with the appropriate role publishes it.
- The system must log who approved each block and when, alongside the prompting context that produced it.
- Provide configurable approval flows (e.g., editor → design lead → compliance) leveraging Drupal’s workflow module where available.
- Generate a publish-ready checklist (asset licenses confirmed, accessibility checks passed, theme tokens applied) before enabling the final publish action.
- Support role-based access control so that certain users can only view or comment on AI suggestions without executing changes.

## Accessibility and Compliance

- Enforce WCAG 2.1 AA standards by validating color contrasts, heading hierarchy, alternative text, and link semantics within each block.
- Require explicit alt text (manual or AI-suggested) before an image block can be approved.
- Flag potential compliance risks (e.g., unverified claims, missing legal disclaimers) and route them to designated reviewers.
- Provide exportable audit logs capturing AI prompts, generated outputs, manual edits, and reviewer approvals for each publishing cycle.
- Ensure generated content respects localization settings (language, date formats) and allow editors to request localized variants per block.
