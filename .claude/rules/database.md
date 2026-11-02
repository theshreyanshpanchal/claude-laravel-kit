# Database & Eloquent

## Schema design

- Design for scale and reliability: correct column types and lengths, `unsigned` foreign keys via `foreignId()->constrained()`, explicit `onDelete` / `onUpdate` behavior.
- Add indexes on every column used in `where`, `orderBy`, joins or lookups. Use composite indexes when queries filter on several columns together.
- Use unique constraints at DB level for anything that must be unique (not only validation).
- Use `decimal` for money (never float). Store timestamps in UTC.
- Store enum-backed columns as `unsignedTinyInteger` (or `unsignedSmallInteger`) with a PHP int-backed enum cast.
- Use soft deletes (`softDeletes()` + `SoftDeletes` trait) only where records must be recoverable or kept for history (orders, users, invoices). Remember unique indexes must account for soft-deleted rows.
- Normalize sensibly; denormalize only with a stated reason (e.g. reporting performance).

## Migrations, seeders, factories

- One migration per logical change. Never edit a migration that may already have run in shared environments; add a new one.
- Every model gets a factory with realistic fake data and useful states (e.g. `->cancelled()`).
- Seeders create a usable demo dataset; `DatabaseSeeder` calls them in the right order.
- `php artisan migrate:fresh --seed` must always work.

## Models

- Define `$fillable` (never `$guarded = []`).
- Use `casts()` for enums, dates, booleans, JSON, decimals.
- Define every relationship with return types (`HasMany`, `BelongsTo`, …).
- Use **local scopes** for reusable query conditions (`scopeActive`, `scopeForUser`).
- Use **accessors / mutators** (`Attribute::make()`) for formatting or normalizing values, not for heavy logic or queries.
- Hide sensitive attributes (`$hidden`).

## Queries

- No N+1: always eager load relations that will be used (`with()`, `load()`, `withCount()`). Consider `Model::preventLazyLoading()` in non-production.
- Select only the columns needed when data sets are large.
- Never query inside loops. Use `whereIn`, bulk `insert` / `upsert`, or relation loading instead.
- Use `chunkById()` / `lazyById()` / cursor for large data sets.
- Paginate list endpoints (`paginate()` or `cursorPaginate()` for large tables).
- Use `exists()` instead of `count() > 0`; `firstOrFail()` / `findOrFail()` where not-found should be a 404.
- Use aggregate queries in the DB (`sum`, `count`, `withSum`) instead of loading rows and computing in PHP.
- Avoid raw SQL unless necessary; if used, always use bindings.

## Transactions

- Wrap any operation that writes to more than one table, or reads-then-writes, in `DB::transaction()`.
- Use `lockForUpdate()` for stock, balances, counters or anything with race conditions.
- Dispatch jobs/events that depend on committed data `afterCommit()`.
- Do not catch exceptions inside the transaction closure just to hide them; let it roll back and handle the error outside.
