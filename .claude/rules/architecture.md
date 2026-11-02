# Architecture & Structure

## Layers and responsibilities

- **Controller** – Thin. Receives a Form Request, calls a service, returns a Resource / response. No business logic, no queries. Aim for under ~100–150 lines.
- **Service** (`app/Services`) – Business logic and orchestration. One service per domain area (e.g. `OrderService`). Aim for under ~300 lines; split by responsibility if larger.
- **Model** – Relationships, casts, scopes, accessors/mutators. No HTTP or request logic.
- **Form Request** – Validation and request-level authorization.
- **API Resource** – Shapes output. Never return models directly.
- **Enum** (`app/Enums`) – Fixed sets of values (statuses, types).
- **Action** classes (`app/Actions`) – Optional, only for a single complex operation reused in several places.

## Contracts (interfaces)

- Use a contract (`app/Contracts`) only when there are, or will clearly be, multiple implementations (e.g. payment gateways, notification channels), or when it makes a dependency swappable for tests.
- Bind contracts to implementations in a service provider.
- Do not create an interface for every service by default.

## Traits and helpers

- **Trait** – Only for behavior genuinely shared by 2+ classes (e.g. `ApiResponse`, `HasUuid`). Keep traits small and focused.
- **Helpers** – Avoid global helper functions. Prefer a trait, a small support class, or a service. If a helpers file is truly needed, autoload it via `composer.json` and keep it minimal.

## Dependency injection

- Inject dependencies through constructors (use PHP constructor property promotion with `readonly` where possible).
- Avoid facades inside services where injection is cleaner; facades are fine in controllers, jobs and simple cases.

## General

- Use `declare(strict_types=1);` if the existing codebase does; otherwise match the codebase.
- Type every parameter, property and return value.
- Use `final` for classes not meant to be extended (services, actions) if consistent with the codebase.
- One class per file. Clear, descriptive names (`OrderService::cancel()`, not `OrderService::process2()`).
