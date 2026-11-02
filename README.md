# Claude Laravel Kit

My Laravel coding standards for Claude Code. Drop the `.claude` folder into any Laravel project and Claude Code follows these rules automatically.

## What's inside

```
.claude/
├── CLAUDE.md                     # Workflow (understand → plan → build → verify) + self-review checklist
├── rules/
│   ├── architecture.md           # Controllers, services, contracts, traits, DI, file size
│   ├── database.md               # Schema, indexes, migrations, seeders, factories, Eloquent, N+1, transactions
│   ├── api.md                    # Routes, naming, requests, resources, response format, auth, RBAC, middleware
│   ├── async-and-events.md       # Queues, scheduler, events, observers, caching
│   ├── code-quality.md           # Enums, match, collections, error handling, comments, env, security
│   ├── testing.md                # When and how to test
│   ├── blade.md                  # Loads only when working on Blade views
│   └── vue.md                    # Loads only when working on Vue / JS files
└── skills/
    └── postman-collection/
        └── SKILL.md              # Run with /postman-collection
BACKUP-PROMPT.md                  # Paste-in version if the folder can't be used
```

## Setup in a new codebase (2 minutes)

From the project root:

**Mac / Linux**
```bash
git clone https://github.com/<your-username>/claude-laravel-kit.git /tmp/claude-laravel-kit
cp -r /tmp/claude-laravel-kit/.claude .
echo ".claude/" >> .git/info/exclude
```

**Windows (PowerShell)**
```powershell
git clone https://github.com/<your-username>/claude-laravel-kit.git $env:TEMP\claude-laravel-kit
Copy-Item -Recurse $env:TEMP\claude-laravel-kit\.claude .
Add-Content .git\info\exclude ".claude/"
```

The last line keeps the folder out of git **without** changing their `.gitignore`. Skip it if you want to commit the standards with your work.

> If the project already has a `.claude` folder, copy only the `rules` and `skills` folders into it, and paste the contents of `CLAUDE.md` at the end of their existing one.

## Check it loaded

Start Claude Code in the project and ask:

```
What coding standards and rules are you following for this project?
```

It should summarize the workflow and the rules. You can also run `/memory` to see the loaded files.

## Using it in the practical round

1. Paste the task and say: **"Plan this first. Don't write code until I approve the plan."**
2. Review the plan (schema, endpoints, classes). Adjust, then approve.
3. Let it build step by step. Commit after each step.
4. Read every change. Ask "why?" on anything you can't explain yourself.
5. At the end: **"Run the self-review checklist and fix anything that fails."**
6. Then: **`/postman-collection`**
7. Finally: **"Update the README with setup steps, decisions, trade-offs and what you'd improve with more time."**

## Clean up after (on someone else's machine)

```bash
rm -rf .claude /tmp/claude-laravel-kit
```

Then run `/logout` in Claude Code.
