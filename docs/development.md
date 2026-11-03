# Development and running

## Requirements

- Node.js compatible with the installed Next.js version.
- `pnpm` or npm (the repository includes `pnpm-lock.yaml` and `package-lock.json`).
- A PostgreSQL database reachable over TLS. The application connects with `ssl: 'require'`.

## Configure the database

Create a root-level `.env.local` file (do not commit it) and set:

```dotenv
POSTGRES_URL=postgresql://USER:PASSWORD@HOST:PORT/DATABASE
```

Use the connection string provided by your PostgreSQL host. `POSTGRES_URL` is used by authentication, data access, server actions, and the seed/query route handlers. The application does not include a local database service or migrations.

## Install, seed, and run

From the repository root:

```bash
pnpm install
pnpm dev
```

For npm-based CI, install the exact versions recorded in `package-lock.json` with `npm ci`.

With the development server running, initialize the database by opening `http://localhost:3000/seed` once. The endpoint creates the required tables and inserts the placeholder users, customers, invoices, and revenue.

Sign in with the seeded development account:

- Email: `user@nextmail.com`
- Password: `123456`

The seed endpoint is unauthenticated and is intended only for development setup. Do not expose it as an unrestricted production endpoint.

## Validation and production

```bash
pnpm lint
pnpm build
pnpm start
```

`pnpm start` serves the production build and therefore requires `pnpm build` to have completed first. Configure `POSTGRES_URL` in the environment for both build/runtime environments where database-backed routes are used.

## Hosting note: GitHub Pages

GitHub Pages serves static files; it does not run a Next.js server or provide a PostgreSQL connection. A `package-lock.json` can support npm-based dependency installation in CI, but it does not make this app deployable to GitHub Pages as-is. The dashboard depends on server rendering, Auth.js, server actions, and the `/seed` and `/query` route handlers. Deploy it to a host that supports a Next.js server, or first redesign it as a static frontend backed by separately hosted APIs and authentication.

## Project scripts

| Command | Purpose |
| --- | --- |
| `pnpm dev` | Start Next.js in development mode with Turbopack |
| `pnpm lint` | Run ESLint |
| `pnpm build` | Create the production build |
| `pnpm start` | Serve the production build |
