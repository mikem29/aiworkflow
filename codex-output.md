Added a technical specification covering AI-managed block storage, styling governance, and interaction safeguards for the IndieMade builder.

**Spec Details**
- Documented the `ai_assistant_html_block` lifecycle, permissions, revisioning, and reuse model so AI can create/edit/unpublish without delete rights while preserving audit trails (`IndieMade/Features/AI Website builder/requirements/ai_block_management_spec.md:1`).
- Defined HTML sanitization, accessibility linting, and manifest-driven prompts to keep generated markup within theme constraints (`IndieMade/Features/AI Website builder/requirements/ai_block_management_spec.md:23`).
- Outlined CSS workflow using a theme manifest, approved utility requests, and an isolated AI style library compiled into scoped assets (`IndieMade/Features/AI Website builder/requirements/ai_block_management_spec.md:36`).
- Established JavaScript behavior catalog, attachment model, and validation schema to gate AI access to pre-approved libraries only (`IndieMade/Features/AI Website builder/requirements/ai_block_management_spec.md:55`).
- Mapped future multi-assistant roles plus operational logging, rate limiting, and recovery considerations for governance (`IndieMade/Features/AI Website builder/requirements/ai_block_management_spec.md:74`).

Next: 1) Review the spec with design/dev stakeholders to confirm the manifest workflow and behavior catalog align with Drupal theme practices before implementation.