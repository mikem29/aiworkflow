# AI Website Builder: Stylesheet Management Specification v2

## Executive Summary

This update overturns the controlled library-only approach and authorizes the AI assistant to create and manage raw CSS so it can deliver fully bespoke layouts. The AI writes styles directly while guardrails enforce file safety, CSS validity, and operational governance. Two storage options are supported—a jailed filesystem workspace and a database-backed stylesheet vault—both fronted by the same validation and publishing pipeline.

## 1. CSS Authoring and Publishing Flow

1. **Prompt Intake**: Builder UI captures the customer request and context (blocks, branding, tokens).
2. **AI Drafting**: AI generates raw CSS plus metadata (intended selectors, purpose, dependencies).
3. **Guardrail Validation** (see Section 3). Draft must pass before it can be stored.
4. **Quarantined Storage**: Valid drafts are written to the selected storage backend (filesystem or database).
5. **Publish Ticket**: Each stylesheet version creates a publish ticket with audit data.
6. **Activation**: Upon approval or auto-approval rules, CSS is promoted to the serving layer and hot-reloaded into the customer site preview.

## 2. Storage Strategies

### 2.1 Filesystem Sandbox (default)
- **Path**: `sites/<site-id>/ai_css/`
- **Extensions**: `.css` only; enforced by middleware before write.
- **Filesystem Guardrails**:
  - Chroot/overlay mount restricts access to the sandbox.
  - Non-blocking checksum monitor ensures file has not been tampered with post-write.
  - Separate read/write credentials for AI service user.
- **Versioning**: Naming scheme `block-<slug>-<timestamp>.css` plus manifest entry for provenance.

### 2.2 Database Vault (optional / enterprise)
- **Storage**: `ai_stylesheets` table or document collection with binary/text column.
- **Benefits**: Centralized auditing, transactional updates, easy replication.
- **Serving**: On publish, assets are materialized to CDN edge or streamed via signed URLs.
- **Fallback**: If database write fails, pipeline retries with filesystem sandbox.

## 3. Guardrails and Validation

### 3.1 CSS Validation Stack
- **MIME Enforcement**: Reject payloads that do not parse as CSS.
- **AST Parsing**: Run through a strict CSS parser (csstree/postcss) to guarantee syntactic validity.
- **Rule Allowlist**: Deny imports, behavior URLs, and banned properties (e.g., `expression`, `behavior`).
- **Token Compliance**: Check optional design token references for existence; flag missing ones.
- **Selector Budget**: Limit number of selectors per file and depth to avoid runaway specificity.

### 3.2 Security Controls
- **Static Analyzer**: Detects suspicious patterns (`url(javascript:)`, external domains).
- **Rate Limiting**: Per-site and per-session caps; escalating backoff on repeated failures.
- **Size Caps**: Reject files larger than 75KB unless elevated permissions supplied.
- **Roles and Approval**: Auto-approve low-risk utilities, route layout-level changes to human reviewer.

### 3.3 Observability
- Collect validation metrics, rejection reasons, and time-to-publish for telemetry dashboards.

## 4. Serving and Integration

- **Manifest**: `ai_css_manifest.json` lists active files with hash, scope (block/page/global), and load priority.
- **Dynamic Load**: Builder injects the published CSS bundle for active blocks via `<link>` tags or consolidated bundle.
- **Rollback**: Manifest tracks previous versions for instant downgrade; stale files kept for 7 days.
- **Preview Sync**: Hot reload in the drafting preview, delayed deploy to production until publish event.

## 5. Governance and Auditability

- **Audit Log**: Store user/AI identity, prompt context, hash, validator results, and approvals.
- **Diff Review UI**: Human reviewers compare proposed vs. previous CSS with syntax highlighting.
- **Auto Cleanup**: Weekly job scans for unused CSS (no manifest reference) and archives it.
- **Incident Response**: Toggle to freeze AI writes site-wide while allowing human overrides.

## 6. Implementation Phases

### Phase 1: Enable Safe Writes
- Build CSS validation service and integrate with AI output pipeline.
- Provision filesystem sandbox and write-only credentials.
- Generate manifest file and ensure builder loads CSS per manifest.

### Phase 2: Operational Guardrails
- Implement audit UI, approval workflows, and metrics dashboards.
- Add automated cleanup scripts and rollback tooling.
- Introduce optional database vault storage for enterprise tenants.

### Phase 3: Optimization and Learning
- Apply ML-based linting for style consistency recommendations.
- Prefetch commonly used AI CSS snippets based on site analytics.
- Benchmark performance and auto-tune bundle splitting and caching.

## 7. Key Decisions

- AI retains full creative control of CSS with non-blocking human checks.
- Validation is mandatory, but approval can be policy-driven (auto vs. manual).
- Storage backend is pluggable; both options share the same manifest-driven delivery.
- Security posture focuses on sandboxing plus semantic CSS validation, not pre-fab templates.

## 8. Risks and Mitigations

- **Malformed CSS** → Strict parsing and manifest gating prevent runtime failures.
- **Malicious Payloads** → Sandbox, rule allowlists, and audit trails trace activity.
- **Performance Regression** → Bundle budget alerts and cleanup jobs limit bloat.
- **Reviewer Bottlenecks** → Auto-approval tiers and workload metrics maintain velocity.

## 9. Open Questions

- Should advanced tenants define custom allowlists/denylists?
- How are JS-influenced interactions (e.g., animation triggers) represented without compromising safety?
- What SLAs are required for CSS publish latency?

