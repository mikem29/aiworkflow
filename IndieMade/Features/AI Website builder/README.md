# AI Website Builder

This folder collects the requirements and research underpinning the Drupal-based AI page builder.

- `requirements/` — Vision, functional requirements, and technical considerations for the AI-assisted page builder.

## Media Style and Asset Management Enhancements

### Phase I — Drupal Media Manager Upgrade

- Provide a media-library modal that lets editors upload, browse, and reuse images without leaving the assistant workspace.
- Surface existing Drupal image styles (scale, crop, responsive variants) so editors can preview, duplicate, or tweak them before attaching assets.
- Allow creation of net-new image styles backed by Drupal’s Image Styles configuration, including governance so site builders approve additions.
- Offer inline adjustments (crop, rotate, focal point, basic color tweaks) with audit trails that map back to the Media entity revision.
- Capture rich metadata on every image (alt text, focal subject, people/objects detected, usage rights, campaign tags) so the AI can retrieve “the profile shot with the paint brush.”
- Auto-suggest metadata by running AI captioning/tagging while keeping human approval in the flow.
- Store assets as Drupal Media entities with taxonomy-based tagging and expose them to conversational queries from the assistant.

### Recommended Drupal Image Style & Media Plugins

- **Image Styles (Core)** — Base pipeline for scale/crop effects; ensure the assistant reads from and writes to these configurations.
- **Responsive Image (Core)** — Delivers breakpoint-aware variants; integrate so AI-selected assets honor responsive rules.
- **Focal Point** — Lets editors specify the visual center so automated crops keep key subjects in frame.
- **Image Widget Crop** — Adds inline cropping UI during upload and edit flows; aligns with the modal editing requirement.
- **Media Library Edit** — Enables quick adjustments (replace file, edit metadata) directly within the modal.
- **ImageAPI Optimize** — Handles compression pipelines (ImageMagick, WebP) to keep generated derivatives lightweight.
- **Image Effects** — Adds additional style transformations (watermarks, color adjustments) that AI-driven presets can invoke.

### Phase II — AI-Assisted Generation and Search

- Integrate an AI image generator (e.g., Google Gemini) as an optional add-on once governance, licensing, and cost controls are defined.
- Support AI-authored metadata enrichment (titles, descriptions, alt text, keywords) with confidence scores so reviewers can accept or revise.
- Persist text embeddings for captions/metadata to enable semantic image search; start with Drupal Search API + taxonomy, then evaluate a vector store when the asset library grows.
- Expand conversational commands so editors can request transformations (“lighten the background,” “apply the hero banner style”) that map to predefined image style recipes.
- Log generator prompts, variants, and approvals to maintain compliance and allow rollback to the original upload.
