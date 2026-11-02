# API Design

## Routes

- Routes in `routes/api.php`, versioned with a prefix: `/api/v1/...`.
- RESTful, plural, kebab-case nouns: `GET /api/v1/orders`, `POST /api/v1/orders`, `GET /api/v1/orders/{order}`, `PATCH /api/v1/orders/{order}`, `DELETE /api/v1/orders/{order}`.
- Non-CRUD actions as sub-resources or clear verbs: `POST /api/v1/orders/{order}/cancel`.
- Use `Route::apiResource()` where it fits, route model binding, and named routes (`orders.index`).
- Group routes by middleware (`auth:sanctum`, throttle) and prefix. No closures in route files.

## Requests and validation

- Every write endpoint uses a Form Request (`StoreOrderRequest`, `UpdateOrderRequest`).
- Use array-style rules, `Rule::enum()`, `Rule::unique()->ignore()`, `exists:` checks.
- Use `authorize()` or policies for permission checks.
- Use `$request->validated()` (or `safe()`) only. Never pass `$request->all()` to models.

## Responses

- Always return data through API Resources (`OrderResource`, `OrderResource::collection()`). Only expose the fields the client needs. Use `whenLoaded()` for relations.
- Use one consistent response structure everywhere, via an `ApiResponse` trait or a response helper class:

```json
{
  "success": true,
  "message": "Order created successfully.",
  "data": { },
  "meta": { }
}
```

Error / validation:

```json
{
  "success": false,
  "message": "The given data was invalid.",
  "errors": { "field": ["Error message"] }
}
```

- Paginated lists include pagination info in `meta`.
- Correct status codes: 200 OK, 201 Created, 204 No Content, 400, 401, 403, 404, 409 Conflict, 422 Validation, 429, 500.

## Authentication & authorization

- Use Laravel Sanctum for API token auth unless the task says otherwise.
- Use Policies for model-level authorization (`$this->authorize()` / `can` middleware).
- **RBAC only if the task asks for roles/permissions.** If asked, prefer `spatie/laravel-permission` (ask before installing) or a simple roles table with a Gate/Policy layer, plus seeders for default roles.

## Middleware

- Use middleware for cross-cutting concerns: auth, throttling, forcing JSON responses, locale, logging. Not for business logic.
- Register middleware the way the installed Laravel version expects.
- Apply rate limiting on auth and write-heavy endpoints.
