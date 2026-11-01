# ComRes

ComRes is a community equipment-sharing application for Community Resource Network SA. It lets people browse equipment, check availability, and request a paid hire, a temporary free loan, or a permanent donation.

## What the application supports

- Requesters can register, browse equipment, make and track requests, cancel eligible requests, save a payment method for an approved request, and leave feedback or reviews.
- Approved community organisations can list equipment and respond to requests for their items.
- Administrators can manage equipment and blackout dates, review requests and organisation applications, oversee fulfillment, and handle eligible deposit charges and refunds.
- Notifications, request-status history, and fulfillment history help users follow each request.

## Technology

- Next.js 16 App Router, React 19, and TypeScript
- Tailwind CSS 4
- Neon PostgreSQL through `@neondatabase/serverless`
- Stripe for card setup and deposit-payment workflows
- Vercel Blob for image uploads

## Set up locally

### Prerequisites

- Node.js 20.9 or later
- npm
- A PostgreSQL database configured with the project schema
- Service credentials for the features you want to exercise (Stripe and Vercel Blob)

Install the locked dependencies from the project root:

```powershell
npm ci
```

Copy the placeholder template and configure its values privately:

```powershell
Copy-Item .env.example .env.local
```

Edit `.env.local` with your database URL, a private `AUTH_SECRET`, and any required Stripe and Blob credentials. `NEXT_PUBLIC_SITE_URL` can be `http://localhost:3000` for local development. Never put real credentials in `.env.example`, documentation, or the handover ZIP.

Start the development server:

```powershell
npm run dev
```

Open [http://localhost:3000](http://localhost:3000). Database-backed pages and external-service workflows need their services configured. The application does not apply database migrations during startup; review the schema and migration cautions in [HANDOVER_README.md](./HANDOVER_README.md) before any database setup or maintenance.

## Check the project

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

## Project layout

- `app/` — application pages and API route handlers
- `components/` — shared interface components
- `lib/` — authentication, database access, request/payment workflows, and tests
- `db/schema.sql` — PostgreSQL schema
- `scripts/` — schema and migration utilities; review each script before use
- `public/` — static assets
- `docs/superpowers/` — design specifications and implementation plans

See [HANDOVER_README.md](./HANDOVER_README.md) for a fuller setup guide and [ComRes_Deliverables_and_Scope_Guide.docx](./ComRes_Deliverables_and_Scope_Guide.docx) for the deliverables index.
