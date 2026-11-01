# ComRes Developer Handover

ComRes is a community equipment-sharing application for Community Resource Network SA. The application supports equipment browsing and availability, paid-hire and free-loan requests, permanent donations, requester accounts, approved organisation listings, and administrator workflows. Request workflows include approval, cancellation, fulfillment history, notifications, and Stripe card/deposit-payment actions.

## Requirements

- Node.js 20.9 or later
- npm
- A PostgreSQL database prepared with the current ComRes schema
- Stripe test credentials and a webhook secret to exercise payment workflows
- A Vercel Blob token to exercise image uploads

The project uses Next.js 16, React 19, TypeScript, Tailwind CSS 4, Neon PostgreSQL (`@neondatabase/serverless`), Stripe, and Vercel Blob.

## Install

From the project root:

```powershell
npm ci
```

This installs the dependency versions recorded in `package-lock.json`.

## Configure local services

Copy the safe placeholder template:

```powershell
Copy-Item .env.example .env.local
```

Edit `.env.local` locally and replace the placeholders as needed:

| Variable | Purpose |
| --- | --- |
| `DATABASE_URL` | PostgreSQL connection string used by the application. |
| `AUTH_SECRET` | Secret used to sign application session tokens. |
| `STRIPE_SECRET_KEY` | Server-side Stripe key; use an authorized test-mode key for development. |
| `STRIPE_WEBHOOK_SECRET` | Validates Stripe webhook deliveries. |
| `BLOB_READ_WRITE_TOKEN` | Enables image uploads to Vercel Blob. |
| `NEXT_PUBLIC_SITE_URL` | Public site origin used for the Stripe card-setup return URLs; use `http://localhost:3000` locally. |

The example values are placeholders, not credentials. Keep real values in `.env.local` or an authorized secret manager. Do not send `.env.local` or other environment files to reviewers or include them in the handover archive.

The final five entries in `.env.example` are optional maintenance/migration settings. They are not needed for normal application startup:

| Variable | Use |
| --- | --- |
| `NEON_API_KEY` | Neon API credential for the payment-backup verification utility and guarded payment migration. |
| `NEON_PROJECT_ID` | Identifies the Neon project for payment-backup verification and the guarded payment migration. |
| `PAYMENT_MIGRATION_EXPECTED_PROJECT_NAME` | Expected non-production project name checked by the guarded payment migration. |
| `PAYMENT_MIGRATION_EXPECTED_DATABASE` | Expected database name checked against the connection URL by the guarded payment migration. |
| `ALLOW_PAYMENT_MIGRATION` | The template sets this to `false`. The migration requires `ALLOW_PAYMENT_MIGRATION=true` in the process environment as an explicit opt-in. |

The guarded payment migration independently validates a designated target. Do not enable it or substitute a different target without explicit authorization. A value in `.env.local` alone does not provide the process-environment opt-in.

## Database preparation

For a **new, empty PostgreSQL database** that you are authorized to initialize:

1. Create the database through the database provider and put its connection URL in `.env.local` as `DATABASE_URL`.
2. From the project root, run the repository's schema-setup script:

   ```powershell
   node scripts/apply-schema.mjs
   ```

   This script reads `.env.local`, applies `db/schema.sql`, then applies `add-equipment-listing-fields.sql`, `add-booking-cancellation-history.sql`, `add-booking-fulfillment.sql`, and `add-deposit-charge-attempts.sql`. It changes database state; run it only against the intended empty database. It is not part of `npm run dev`, `npm run build`, or `node --test`.
3. Configure `AUTH_SECRET` before starting the application. Configure Stripe and Blob credentials only when exercising those integrations.

The payment migration for `booking_payments` and `booking_payment_attempts` is separate from fresh-schema setup. `scripts/apply-payment-migration.mjs` has strict target checks tied to a designated backup branch and is not a general-purpose migration for a new database. Treat it as a separately authorized maintenance operation; do not run it as part of routine installation.

### First administrator account

The public registration endpoint creates requester or organisation accounts; it does not allow registering as an administrator. The schema-setup script creates tables only and does not create an administrator. The existing seed script would create a sample administrator and demo data, but it is intentionally excluded from this handover because it contains default account credentials. Therefore, a new installation needs an authorized administrator or database operator to provision the first administrator through an approved process outside this package. No first-admin bootstrap procedure is provided here because the submitted application has no supported self-service admin-creation flow.

The handover ZIP also omits `scripts/reset-db.mjs`, which truncates user, equipment, request, booking, and blackout data. Do not restore or use reset/seed utilities in a shared or production environment.

For any other database operation, first confirm the target, inspect the exact script and its safeguards, and make an appropriate backup. Do not run setup or migration utilities against shared or production databases without explicit authorization.

## Run locally

Start the development server:

```powershell
npm run dev
```

Open [http://localhost:3000](http://localhost:3000). Pages that use live inventory, accounts, or external services require those services to be configured and the database schema to be current.

## Test, lint, and build

Run the Node.js built-in test files:

```powershell
node --test
```

Run ESLint:

```powershell
npm run lint
```

Create and serve a production build:

```powershell
npm run build
npm run start
```

`package.json` defines the `dev`, `build`, `start`, and `lint` npm scripts; tests are run directly with Node.js.

## Project map

| Location | Contents |
| --- | --- |
| `app/` | Next.js pages and API route handlers for authentication, equipment, requests, organisations, administration, feedback, reviews, notifications, uploads, and Stripe webhooks. |
| `components/` | Shared interface components, including navigation, equipment cards, availability calendars, and request-history/actions. |
| `lib/` | Authentication, database access, availability, request/payment workflows, and unit tests. |
| `db/schema.sql` | PostgreSQL schema. |
| `scripts/` | Database setup, migration, and verification utilities; review each before use. |
| `docs/superpowers/` | Design specifications and implementation plans. The organisation-account plan retains its AI-use disclosure. |
| `public/` | Static assets. |
| `Requirement.png` | Requirements reference image. |
| `.env.example` | Placeholder-only local configuration template. |
| `ComRes_Deliverables_and_Scope_Guide.docx` | Deliverables and scope index. |

The final handover ZIP contains the project source, tests, documentation, assets, setup instructions, and `.env.example`. It excludes `.env.local`, Git metadata, dependencies, generated build/cache files, AI-tool instruction files (`AGENTS.md` and `CLAUDE.md`), and the seed/reset scripts. Those instruction files remain in the original project; they are not needed by a developer receiving the application.
