# Code Quality

## Enums

- Use PHP backed enums with **integer** values for statuses and types:

```php
enum OrderStatus: int
{
    case Pending = 1;
    case Paid = 2;
    case Cancelled = 3;

    public function label(): string
    {
        return match ($this) {
            self::Pending => 'Pending',
            self::Paid => 'Paid',
            self::Cancelled => 'Cancelled',
        };
    }
}
```

- Cast enum columns on the model. Validate with `Rule::enum()`. Expose both value and label in Resources when helpful.
- Put enum-related logic on the enum itself (`label()`, `canTransitionTo()`), not scattered in services.

## Control flow

- Prefer `match` over `switch` and long `if/elseif` chains.
- Use early returns and guard clauses instead of deep nesting.
- No magic numbers or strings; use enums, constants or config.

## Collections over arrays

- Prefer Laravel Collection methods (`map`, `filter`, `pluck`, `keyBy`, `groupBy`, `sum`, `each`, `firstWhere`, `contains`) over `array_*` functions and manual loops, when working with collections.
- Do the filtering/aggregating in the database when possible; use collections for data already loaded.

## Error handling

- Use `try/catch` only where you can handle the error meaningfully (external API calls, file operations, payment calls). Do not wrap everything.
- Throw specific custom exceptions for domain errors (`InsufficientStockException`) and render them consistently (via the exception handler in `bootstrap/app.php` for Laravel 11+, or a `render()` method).
- Never swallow exceptions silently. Log with context (`Log::error('...', ['order_id' => $id])`).
- Never expose stack traces or internal messages to API clients; use the common error response format.
- Return a 404 for missing models (`findOrFail`, route model binding), 403 for authorization failures, 409 for invalid state changes, 422 for validation errors.

## Formatting & readability

- Follow PSR-12 and Laravel conventions. Run `./vendor/bin/pint` before finishing.
- One blank line between methods and logical blocks. No trailing whitespace. No unnecessary blank lines.
- Descriptive names for variables and methods. No abbreviations like `$o`, `$tmp`, `$data2`.
- Keep methods short and focused (ideally under ~30 lines).

## Comments

- Comment only *why*, never *what*. Code should explain itself.
- No generic or obvious comments (`// get users`, `// constructor`).
- Short PHPDoc only where types cannot express it (e.g. `@return Collection<int, Order>`) or for complex business rules.

## No dead code

- No unused imports, variables, methods, classes, routes or migrations.
- No commented-out code. No leftover debug calls. No TODOs.
- Remove default Laravel scaffold files that are not used, only if safe.

## Configuration & environment

- Never call `env()` outside config files. Read through `config()`.
- Add every new variable to `.env.example` with a safe placeholder and a short comment if not obvious. Never commit real secrets.
- Group related variables (DB, mail, queue, third-party services).

## Security

- Mass assignment protected (`$fillable`), validated input only.
- Authorize every action (policies/gates).
- Escape output in Blade (`{{ }}`); use `{!! !!}` only for trusted content.
- Hash passwords; hide sensitive fields; never log secrets or tokens.
