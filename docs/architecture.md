# Project architecture

This repository is a Next.js App Router dashboard built with TypeScript, React, and Tailwind CSS. It is a learning-project starter, not a general-purpose application framework.

## Application structure

- `app/` contains route segments, layouts, pages, and route handlers.
- `app/ui/` contains shared presentation components grouped by dashboard, invoices, and customers.
- `app/lib/` contains PostgreSQL data access (`data.ts`), server actions (`actions.ts`), shared types (`definitions.ts`), formatting helpers (`utils.ts`), and seed data (`placeholder-data.ts`).
- `auth.ts` configures Auth.js credentials authentication; `auth.config.ts` contains shared authorization and sign-in routing.
- `proxy.ts` applies the Auth.js authorization callback to matched requests. Dashboard routes require a logged-in user.
- `public/` contains static images and icons used by the interface.

## Routes

| Route | Responsibility |
| --- | --- |
| `/` | Public course landing page |
| `/login` | Credentials sign-in form |
| `/dashboard` | Overview cards, revenue chart, and latest invoices |
| `/dashboard/invoices` | Searchable, paginated invoice list |
| `/dashboard/invoices/create` | Invoice creation form |
| `/dashboard/invoices/[id]/edit` | Invoice editing form; missing invoices use `not-found.tsx` |
| `/dashboard/customers` | Customer summary table |
| `/seed` | GET route that creates tables and inserts placeholder data |
| `/query` | GET example query for invoices with an amount of 666 |

`app/dashboard/layout.tsx` supplies the dashboard navigation shell. The dashboard overview uses `Suspense` with skeleton components while its data loads. Invoice failures are handled by `app/dashboard/invoices/error.tsx`.

## Data and mutation flow

1. Server-rendered pages and UI server components call functions in `app/lib/data.ts`.
2. Those functions issue parameterized SQL with the `postgres` package using `POSTGRES_URL` and TLS (`ssl: 'require'`).
3. Invoice forms submit to server actions in `app/lib/actions.ts`. Actions validate form input with Zod, write to PostgreSQL, revalidate relevant paths, and redirect after successful create/update.
4. `app/seed/route.ts` creates the `users`, `customers`, `invoices`, and `revenue` tables and loads data from `placeholder-data.ts`.

Amounts are stored in cents in the database. Convert/format them through the existing helpers and patterns rather than changing units in only one layer.

## Authentication

Auth.js uses a credentials provider configured in `auth.ts`; credentials are checked against the `users` table and passwords are compared with `bcrypt`. The shared callback in `auth.config.ts` protects `/dashboard` paths and sends unauthenticated users to `/login`. The seed data provides a development-only account; see the [run guide](development.md).
