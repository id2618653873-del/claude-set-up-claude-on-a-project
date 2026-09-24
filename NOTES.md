# NOTES.md

## CLAUDE.md choices

I kept `CLAUDE.md` to four short sections: a one-line project description, Commands, Conventions, and Architecture.

What I included:
- **Commands**: `npm run dev`, `npm test`, `npm run lint` — the three scripts a contributor (or Claude) will run constantly, straight from `package.json`.
- **Conventions**: CommonJS over ES modules (matches `.eslintrc.json`'s `sourceType: "script"` and every existing file), always going through `db/store.js` for data access instead of touching the in-memory array from a route, the route-comment style already used in `routes/users.js`/`routes/health.js`, and the one-file-per-resource pattern for new routes. These are real, checkable rules — not restatements of what's obvious from reading the code once.
- **Architecture**: a few lines on `server.js` as the entry point (and why it guards `app.listen` behind `require.main === module`), one route file per resource, and `db/store.js` as the only data-access layer.

What I deliberately left out:
- Any per-endpoint documentation (routes are few and self-explanatory from the route files themselves — restating them would just go stale).
- Anything from `.env.example` — no secrets or environment details belong in a file that's committed and loaded into every session.
- One-off/task-specific notes ("I'm currently working on X") — those belong in a conversation or a PR description, not in a file every future session reads.

Shorter felt stronger here: the project is small enough that a long `CLAUDE.md` would mostly restate the code rather than add judgment Claude can't get by reading the files itself.

## Permission rules

- **Allow**: `Bash(npm test:*)` and `Bash(npm run lint:*)` — both are safe, read-only-in-effect commands I want to run constantly without a confirmation prompt slowing down every session.
- **Ask**: `Bash(git push:*)` — pushing is not destructive by itself, but I want a chance to glance at what's being pushed before it leaves my machine.
- **Deny**: `Read(./.env)` and `Bash(git push --force:*)`.
  - Without the `.env` deny rule, Claude could read real secrets (API keys, database URLs) straight out of a git-ignored file and potentially echo them into a response, a commit message, or a shared conversation.
  - Without the force-push deny rule, an agentic session could silently overwrite remote history — rewriting or destroying commits other people are relying on — with no confirmation step in between.

## Verification

- `/memory` shows `CLAUDE.md` loaded for the project.
- `/permissions` shows the allow/ask/deny rules from `.claude/settings.json`.
- Asking "How do I run the tests here?" is answered from `CLAUDE.md` (`npm test`) without further explanation needed.
