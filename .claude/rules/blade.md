---
paths:
  - "resources/views/**/*.blade.php"
  - "app/View/**/*.php"
---

# Blade Templates

- Use one main layout (component layout `<x-app-layout>` or `@extends('layouts.app')` with `@section` / `@yield`), matching the codebase.
- Extract repeated UI into Blade components (`resources/views/components`): buttons, inputs, alerts, cards, modals, tables. Use anonymous components for simple markup and class-based components when logic is needed.
- Use `@props` with defaults, and `$attributes->merge()` for class passing.
- Use partials (`@include`) for page sections; `@stack` / `@push` for page-specific scripts and styles.
- No queries or business logic in views. Pass prepared data from the controller (or a view model / view composer if shared across many views).
- Always escape output with `{{ }}`. Use `{!! !!}` only for trusted, sanitized HTML.
- Use `@csrf` and `@method` in forms. Show validation errors with `@error` and keep old input with `old()`.
- Use `@forelse` for lists with an empty state.
- Use named routes (`route('orders.show', $order)`), never hard-coded URLs.
- Use `@can` / `@cannot` for permission-based UI.
- Keep views small and readable, consistent indentation, no inline styles or large inline scripts.
