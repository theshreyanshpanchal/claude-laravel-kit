# Testing

## When to write tests

- If the task asks for tests, write them fully.
- If the task does not mention tests, **ask** during planning. If there is no answer, write a small set (3–6) of feature tests for the main flows, as a lead would.

## How

- Use the framework already installed (Pest or PHPUnit). Default to Pest for new projects if available.
- Feature tests for API endpoints: happy path, validation failure, unauthorized/forbidden, not found, key edge cases (e.g. invalid status change, out of stock).
- Unit tests only for isolated business logic (services, enums, calculations).
- Use `RefreshDatabase`, factories and factory states. No hard-coded IDs.
- Use `Queue::fake()`, `Event::fake()`, `Mail::fake()`, `Notification::fake()`, `Http::fake()` to assert side effects without running them.
- Assert status code, JSON structure (`assertJsonStructure`) and database state (`assertDatabaseHas`, `assertSoftDeleted`).
- Test names describe behavior: `it('prevents cancelling a shipped order')`.
- Run the full suite before finishing; all tests must pass.
