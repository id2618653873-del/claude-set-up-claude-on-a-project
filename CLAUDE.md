# claude-course-starter

A small Express.js REST API (in-memory user store) used as a sandbox project for the Claude Code course.

## Commands

- `npm run dev` — start the API with auto-reload at http://localhost:3000
- `npm test` — run the test suite (Node's built-in test runner + supertest)
- `npm run lint` — check code style with ESLint

## Conventions

- Use CommonJS (`require`/`module.exports`), not ES module `import`/`export` — the rest of the codebase is CommonJS and `.eslintrc.json` sets `sourceType: "script"`.
- Every route handler goes through `db/store.js` for data access — never mutate the in-memory `users` array directly from a route file.
- New endpoints get a one-line `// METHOD /path — purpose` comment above the handler, matching the existing style in `routes/users.js` and `routes/health.js`.
- New resources get their own file in `routes/`, mounted in `server.js` with `app.use("/resource", resourceRoutes)`.

## Architecture

- `server.js` is the entry point: builds the Express app, mounts routes, and only calls `app.listen` when run directly (not when imported by tests).
- `routes/` holds one file per resource (`users.js`, `health.js`); each exports an Express `Router`.
- `db/store.js` is a tiny in-memory data helper — no real database. Data resets on every restart.
- `tests/` contains supertest-based route tests that import `app` from `server.js` without starting a real listener.
