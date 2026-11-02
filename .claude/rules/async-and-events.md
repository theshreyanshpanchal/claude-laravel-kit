# Queues, Scheduler, Events & Observers

Use these only when they solve a real problem in the task. State the reason in one line.

## Queued jobs

- Use a job for slow or external work the user should not wait for: emails, notifications, exports, third-party API calls, heavy processing.
- Jobs implement `ShouldQueue`, are small, and receive IDs or models (serialized models are fine).
- Set `$tries`, `$backoff` and `$timeout`. Implement `failed()` to log or notify.
- Make jobs idempotent (safe to run twice). Use `ShouldBeUnique` where duplicates would cause harm.
- Dispatch with `afterCommit()` when the job relies on data written in a transaction.
- Use a named queue for different priorities if it adds clear value.
- Keep `QUEUE_CONNECTION` in `.env.example` and document `php artisan queue:work` in the README.

## Scheduler

- Define schedules in `routes/console.php` (Laravel 11+) or the console kernel (older versions), matching the installed version.
- Use `withoutOverlapping()` and `onOneServer()` where relevant.
- Scheduled tasks should dispatch jobs or call commands/services, not contain business logic inline.
- Document `php artisan schedule:work` (local) or the cron entry in the README.

## Events & listeners

- Use events when one action triggers several independent side effects (e.g. `OrderPlaced` → send email, update stats, notify admin).
- Listeners that do slow work implement `ShouldQueue`.
- Event names in past tense (`OrderPlaced`, `PaymentFailed`).
- Do not use events for core logic that must happen in the same flow; call the service directly.

## Observers

- Use an observer for simple, model-lifecycle side effects (setting a slug or UUID on `creating`, clearing cache on `saved`).
- Do not put important business logic in observers; it becomes hidden. Prefer explicit service calls or events for that.
- Register observers with the `#[ObservedBy]` attribute or a service provider, matching the codebase.
- Be aware observers do not fire on mass updates/deletes (`Model::where()->update()`).

## Caching

- Cache expensive, frequently read, rarely changed data (`Cache::remember`) with a clear TTL and key naming.
- Invalidate cache when the underlying data changes.
