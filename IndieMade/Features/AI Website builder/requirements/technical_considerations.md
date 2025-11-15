# Technical Considerations

## Drupal Integration Architecture

- Implement the assistant as a custom Drupal module that exposes a side-panel or full-screen UI, leveraging the Layout Builder or Claro admin theme for consistency.
- Use Drupal’s JSON:API or custom REST endpoints to manage blocks, media entities, nodes, and configuration from the assistant front end. Ensure endpoints respect existing permissions.
- Represent AI-generated plans as draft configuration entities so they can be versioned, reviewed, and rolled back without affecting published content.
- Integrate with Drupal’s Workflow/Content Moderation modules to align AI draft statuses with the site’s approval pipeline.
- Provide webhooks or queues for asynchronous operations (e.g., large asset processing, long-running AI calls) to avoid blocking the UI.

## Asset Storage and Retrieval

- Store uploaded files using Drupal’s Media and File entities, inheriting existing storage backends (local filesystem, S3, etc.).
- Maintain metadata taxonomy vocabularies for asset tagging so AI queries can retrieve assets based on theme, campaign, or page goals.
- Cache AI-generated asset descriptions and embedding vectors to improve future retrieval; consider using the Entity API to store AI annotations on Media entities.
- Expose asset selection APIs that return thumbnails, metadata, usage counts, and licensing notes to the assistant interface.
- Synchronize image style definitions via Drupal’s `image_style` configuration entities so the assistant can list, duplicate, and version styles programmatically.
- Bundle common media-focused contributed modules (Focal Point, Image Widget Crop, Media Library Edit, Responsive Image, ImageAPI Optimize, Image Effects) and expose their capabilities through a single media service layer.
- Persist rich metadata (subjects, colors, people, moods, campaign tags) within structured fields and taxonomies; store optional text embeddings alongside to support semantic lookup when enabled.
- Start with Drupal Search API + taxonomy filters for Phase I, but design adapters that can swap in a vector database (e.g., OpenSearch k-NN, Pinecone) when semantic search needs exceed relational queries.

## AI Enablement Strategy

- Use existing LLM APIs (e.g., OpenAI, Anthropic) connected through a server-side proxy that injects Drupal context (site branding guidelines, content types, block schemas).
- Maintain prompt templates that include structured data about available block types, field definitions, and validation rules to prevent unsupported outputs.
- Persist conversation state per page session in Drupal or an auxiliary datastore so AI responses remain contextual across user interactions.
- Introduce guardrails such as schema validation, toxicity filters, and maximum token limits before writing generated content to Drupal entities.
- Collect feedback signals (accept/reject, manual edits) to refine future prompts and optionally retrain fine-tuned models or retrieval indexes.
- Run background AI services (captioning, keyword extraction, alt-text drafting) when new images are uploaded, and store scores that indicate confidence and moderation status.
- Isolate optional Phase II image generation (Google Gemini or comparable) behind feature flags and budget guardrails; generated variants must link back to the originating asset and prompt payload.

## Theming and Style Alignment

- Extract key theme specifications (color palette, typography scale, spacing rules, component class names) into a machine-readable manifest shared with the AI.
- Surface layout constraints (available regions, responsive breakpoints, allowed block combinations) so the assistant proposes valid configurations.
- Provide a reference library of approved block templates with preview HTML/CSS that the AI can reference when generating new blocks.
- Enforce style tokens by validating that generated entity fields include required classes or design system references before saving changes.
- Offer a “theme drift” report whenever generated content deviates from the manifest, and allow designers to update the manifest without code changes.

## Security, Privacy, and Auditability

- Authenticate assistant actions using Drupal user sessions; AI-triggered writes must execute with the current user’s permissions.
- Obfuscate or redact sensitive data before it leaves Drupal for AI processing, following organizational privacy policies.
- Rotate API keys and secrets via Drupal’s configuration management or environment variables; never expose them client-side.
- Log all AI prompts, responses, and resulting entity changes with timestamps and user IDs for compliance tracing.
- Provide rate limiting and error handling to prevent abuse or runaway API consumption.

## Non-Functional Requirements

- **Performance**: Core assistant interactions (loading a draft plan, applying a block update) should complete within 2–3 seconds under normal load.
- **Scalability**: Support concurrent editing sessions per Drupal node without conflicts; rely on locking or conflict resolution for simultaneous block edits.
- **Availability**: Target 99.5% uptime for assistant services tied to Drupal’s availability; degrade gracefully if the AI provider is unavailable.
- **Extensibility**: Allow additional AI tools (e.g., image generation) to be plugged in via a defined interface without rewrites.
- **Observability**: Instrument key flows with metrics and alerts (prompt latency, success/failure counts, rejection reasons).

## Open Questions and Risks

- How will the team curate and maintain the theme manifest that the AI relies upon, and who owns its updates?
- What limits should be placed on asset file size or formats to balance editor needs with storage constraints?
- Which AI vendors meet the organization’s compliance requirements, and what fallbacks exist if quotas are exhausted?
- How should conflicts be resolved when multiple editors request AI changes to the same block simultaneously?
- Is additional training or change management required for editors to trust and effectively use the assistant?
- What criteria (asset volume, search latency, personalization needs) should trigger investment in a dedicated vector store versus continuing with relational metadata queries?
- How will licensing, watermarking, and PII concerns be handled for AI-generated imagery introduced in Phase II?
