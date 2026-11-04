# Frappe framework guidance

## Scope and ownership

This is the customized Frappe v15 framework repository within the parent bench. Shared
changes here can affect ERPNext, POS Awesome, HRMS, Web Sync, and other installed apps.
For an app-specific request, first check whether its controller, hook, or stylesheet is
the appropriate owner. Change framework behavior when the requested behavior belongs
here; do not introduce business-specific defaults into shared primitives.

Use this repository's Git status and configured customized `origin`. Preserve unrelated
edits and public compatibility. Do not replace the fork with upstream source or infer
permission to commit, push, migrate a site, or deploy from a local edit request.

## Code map

- `frappe/desk/`, `frappe/public/`: native Desk pages, controls, client runtime, and assets.
- `frappe/model/`, `frappe/core/`: documents, DocTypes, metadata, and lifecycle behavior.
- `frappe/permissions.py`, `frappe/auth.py`, `frappe/api/`, `frappe/database/`:
  authorization, authentication, API contracts, and persistence.
- `frappe/printing/`, `frappe/website/`, `frappe/integrations/`: shared output and integration paths.
- `ui/`: local `@framework/ui` library. Read `ui/CLAUDE.md` and `ui/PHILOSOPHY.md`
  before component changes. It is not the installed `frappe-ui` package; trace imports
  and consumers before deciding that a component is missing or duplicating one.

## UI direction

The owner prefers the calm, professional simplicity of the customized Product Catalog
and Product Detail in ERPNext: compact identity, clear labels, useful summary figures,
readable tables, and progressively disclosed detail. Apply that intent to the task,
without making every framework screen a product dashboard.

Use existing Desk controls, page actions, typography, theme tokens, translation helpers,
and formatting. Keep global style changes intentional and verify their consumers; scope
screen-specific styling. Avoid e-commerce layouts, decorative badges, oversized imagery,
or a new UI stack for an internal screen. Preserve Russian/Tajik wording, keyboard focus,
accessible names, responsive layouts, and supported theme behavior.

Provide understandable loading, empty, error/retry, and restricted-access states. Ignore
obsolete responses during navigation/search and preserve relevant filter state. Shared
controls must remain usable with existing metadata and calling conventions.

## Framework invariants

- Preserve server-side document and row-level permissions, authentication boundaries,
  submitted/cancelled lifecycle rules, and audit behavior. Hiding a control is not access control.
- Do not weaken a shared API or add broad `ignore_permissions` behavior to make one
  simplified app screen work. Check both allowed and denied paths for permission changes.
- Trace hooks, caches, realtime events, background jobs, and transaction boundaries when
  changing shared behavior. Preserve after-commit semantics where consumers depend on them.
- Keep API response shapes, metadata, filters, pagination, and error behavior compatible
  unless the requested change includes a coordinated consumer update.
- Use parameterized queries and escape dynamic output. Do not add business mutations
  to read-only screens. Avoid per-row database/API lookups when the data can be batched.
- Follow existing migration and patch conventions for schema/data changes; do not silently
  rewrite existing site data or treat a production migration as a diagnostic check.

## Verification and handoff

Read the affected implementation and representative consumers before editing. For a
shared control or API, verify the impacted app paths rather than testing it only in
isolation. Match tests to risk: permissions, document lifecycle, database semantics, and
public contracts need meaningful coverage; a small visual adjustment need not create a suite.

Use the parent bench's existing Python environment and a suitable test site for Frappe
tests. `bench --site <test-site> run-tests --module <module>` is the scoped test pattern.
Inspect `package.json` and existing tooling before selecting frontend checks; the root
asset scripts are `yarn build`, `yarn production`, and `yarn watch`. Use a scoped bench
build when appropriate. The local `ui/` package does not share every root script.

Check desktop/mobile rendering and relevant Russian/Tajik labels for visible changes.
Report changes, affected consumers, completed checks, and remaining runtime steps.
Distinguish a fixture preview from an authenticated test against the running site.
