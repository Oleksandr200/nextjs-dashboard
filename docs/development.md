# Development and running

## Requirements

- Node.js compatible with the installed Next.js version.
- `pnpm` (the repository includes `pnpm-lock.yaml`).
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

## Project scripts

| Command | Purpose |
| --- | --- |
| `pnpm dev` | Start Next.js in development mode with Turbopack |
| `pnpm lint` | Run ESLint |
| `pnpm build` | Create the production build |
| `pnpm start` | Serve the production build |
