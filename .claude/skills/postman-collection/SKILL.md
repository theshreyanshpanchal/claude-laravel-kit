---
name: postman-collection
description: Generate a Postman collection (v2.1 JSON) for this Laravel project's API, with folders, variables, request descriptions and example responses. Use when the user asks for a Postman collection or API documentation export.
---

# Postman Collection Generator

Create a Postman collection for the API routes in this project.

## Steps

1. Run `php artisan route:list --path=api --json` to get every API route. Ignore framework/vendor routes not built for this task (e.g. `sanctum/csrf-cookie`) unless they are used.
2. For each route, read its controller method, Form Request (validation rules) and API Resource (response shape).
3. Build the collection and save it as `postman/<ProjectName>.postman_collection.json` (Postman Collection v2.1 schema).
4. Also save `postman/<ProjectName>.postman_environment.json` with the same variables, so it can be imported directly.
5. Validate the file is valid JSON. Mention the file path in the README.

## Structure

- Collection name = project name. Collection description = short overview, auth instructions and how to use the variables.
- **One folder per resource/module** (e.g. `Auth`, `Orders`, `Products`). Order: Auth first, then the main flows in logical order.
- Request names are human-readable: `List Orders`, `Create Order`, `Cancel Order`.

## Variables

Define collection variables and use them everywhere (never hard-code values):

- `{{BASE_URL}}` – e.g. `http://localhost:8000`
- `{{VERSION}}` – e.g. `v1`
- `{{BEARER_TOKEN}}` – auth token
- Resource IDs as needed: `{{ORDER_ID}}`, `{{PRODUCT_ID}}`

URLs follow the pattern: `{{BASE_URL}}/api/{{VERSION}}/orders/{{ORDER_ID}}`

## Auth

- Set collection-level auth to **Bearer Token** with `{{BEARER_TOKEN}}`. Requests inherit it.
- Public endpoints (login, register) set auth to **No Auth**.
- Add a test script on the Login request that saves the token automatically:

```js
const res = pm.response.json();
if (res?.data?.token) {
    pm.collectionVariables.set("BEARER_TOKEN", res.data.token);
}
```

## Each request

- Headers: `Accept: application/json` and `Content-Type: application/json` (for bodies).
- Body: raw JSON with realistic sample values matching the validation rules.
- Query params for list endpoints (pagination, filters, sort), disabled by default if optional, each with a description.
- **Description** (markdown): what the endpoint does, who can access it, and a validation table:

| Field | Type | Required | Rules |
|---|---|---|---|
| customer_id | integer | Yes | exists:customers,id |

## Example responses

Add **exactly two** saved examples per request, using the project's real response structure:

1. **Success** – correct status code (200 / 201 / 204) with a realistic body.
2. **One error** – the most relevant one: validation (422) for write endpoints, not found (404) for show/update/delete, unauthorized (401) for protected list endpoints.

Do not add examples for every possible status code.
