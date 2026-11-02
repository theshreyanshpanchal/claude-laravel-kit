# Backup prompt

Use only if the `.claude` folder can't be copied. Paste everything below the line as your first message to Claude Code.

---

For this whole task, act as a lead Laravel developer whose code will be reviewed by senior engineers. Follow these standards strictly.

**Workflow:** First read composer.json and the existing structure and match its conventions. Then ask clarifying questions and propose a plan (DB schema with indexes/foreign keys, endpoints, classes, any queue/event/observer usage with a reason). Wait for my approval. Then build in small steps and suggest a git commit after each. Verify with `migrate:fresh --seed`, `route:list` and tests. Finish with a self-review against these rules, then update README.md and .env.example.

**Architecture:** Thin controllers (Form Request in → service → API Resource out). Business logic in services. Contracts only when multiple implementations exist. Traits only for behavior shared by 2+ classes. Constructor injection. Typed everything. Controllers ~150 lines max, services ~300.

**Database:** Scalable schema, `foreignId()->constrained()`, indexes on filtered/sorted columns, DB-level unique constraints, decimal for money. Int-backed PHP enums stored as unsigned tinyint and cast on the model. Soft deletes only where records must be recoverable. Factories with states and seeders for every model. `$fillable`, casts, typed relationships, local scopes, accessors/mutators via `Attribute`.

**Queries:** No N+1 (eager load), no queries in loops, select needed columns, paginate lists, aggregates in DB, `exists()` over `count()`. Multi-table writes in `DB::transaction()`, `lockForUpdate()` for race conditions, dispatch jobs `afterCommit()`.

**API:** `/api/v1`, RESTful plural kebab-case routes, route model binding, named routes, no closures. Form Requests for every write, `validated()` only. API Resources for every response, only needed fields. One response format: `{success, message, data, meta}` and `{success, message, errors}`. Correct status codes. Sanctum auth, policies for authorization, rate limiting. RBAC only if the task asks for it.

**Async:** Jobs for slow/external work with tries, backoff, failed(), idempotent. Scheduler with withoutOverlapping. Events for multiple independent side effects. Observers only for simple lifecycle effects. Use none of these unless they fit.

**Quality:** `match` over switch, early returns, no magic values. Collection methods over array functions. try/catch only where it can be handled; custom domain exceptions rendered consistently; never swallow or expose internals. PSR-12 + Pint. No dead code, unused imports, debug calls, commented-out code or TODOs. Comments only explain *why*. `env()` only in config files.

**Tests:** Ask whether tests are wanted. If unanswered, write 3–6 Pest/PHPUnit feature tests for main flows, validation, auth and edge cases.

**Frontend (if any):** Blade layouts + components, no logic in views, escaped output. Vue 3 `<script setup>`, small components, API calls in services/composables, loading/empty/error states.

**At the end, when I ask for a Postman collection:** v2.1 JSON in `postman/`, one folder per module, variables `{{BASE_URL}}`, `{{VERSION}}`, `{{BEARER_TOKEN}}` and IDs, collection-level Bearer auth, login script that saves the token, request descriptions with a validation table, and exactly two examples per request (one success, one relevant error).
