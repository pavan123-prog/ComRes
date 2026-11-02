# Developer Setup and Technical Handover

This guide is for a developer or operator preparing a local ComRes installation. It explains the setup steps and actions that need special care. For a non-technical introduction, see the [Project Overview](./ComRes_Client_Handover/01_Project_Overview/Project_Overview_and_Evaluation.pdf) and [Deliverables and Scope Guide](./ComRes_Client_Handover/01_Project_Overview/ComRes_Deliverables_and_Scope_Guide_Final.pdf).

## What ComRes does

ComRes lets requesters ask to hire, temporarily borrow, or receive equipment as a donation. Approved community organisations can maintain their own listings and respond to requests. Administrators review organisation applications, manage shared equipment and requests, and oversee equipment handovers and eligible payment actions.

The project code contains these workflows. This package does not include evidence of complete live user journeys, a security assessment, usability testing, or client acceptance. The [scope guide](./ComRes_Client_Handover/01_Project_Overview/ComRes_Deliverables_and_Scope_Guide_Final.pdf) records these limitations.

## What you need

- Node.js 20.9 or later and npm.
- An authorized PostgreSQL database prepared with the ComRes schema.
- Stripe test credentials and a webhook secret to try payment workflows.
- A Vercel Blob token to try image uploads.

The application uses Next.js, React, TypeScript, PostgreSQL through Neon, Stripe, and Vercel Blob. In plain terms, these provide the website, data storage, card-payment services, and image storage.

## Install the project

Open a terminal in the project folder and install the locked package versions:

```powershell
npm ci
```

Copy the safe example settings file:

```powershell
Copy-Item .env.example .env.local
```

Keep `.env.local` private and untracked. Do not send it to reviewers or include it in a ZIP. Never put real passwords, tokens, or service credentials in `.env.example`.

## Set up private service settings

Add only the settings for services you are authorized to use:

| Setting | What it is for |
| --- | --- |
| `DATABASE_URL` | Private connection address for the PostgreSQL database. |
| `AUTH_SECRET` | Private value used to protect signed-in sessions. |
| `STRIPE_SECRET_KEY` | Server-side Stripe key. Use an authorized test-mode key during development. |
| `STRIPE_WEBHOOK_SECRET` | Checks that payment updates came from Stripe. |
| `BLOB_READ_WRITE_TOKEN` | Allows the application to store uploaded images in Vercel Blob. |
| `NEXT_PUBLIC_SITE_URL` | Website address used when returning from card setup; use `http://localhost:3000` locally. |

Five additional settings in `.env.example` are for specific database maintenance tools. They are **not** required to start the application:

| Setting | When it is used |
| --- | --- |
| `NEON_API_KEY` | Lets an authorized maintenance utility contact Neon. |
| `NEON_PROJECT_ID` | Identifies the Neon project used by that utility. |
| `PAYMENT_MIGRATION_EXPECTED_PROJECT_NAME` | Safety check for the specific target of the guarded payment migration. |
| `PAYMENT_MIGRATION_EXPECTED_DATABASE` | Safety check for the database name used by that migration. |
| `ALLOW_PAYMENT_MIGRATION` | Must be explicitly enabled in the process environment before the guarded migration can proceed. The example value is `false`. |

Do not enable a maintenance migration or change its target without explicit authorization. A value saved in `.env.local` does not replace the required process-environment opt-in.

## Prepare a new database

Only follow these steps for a **new, empty database** that you are authorized to initialize. This setup changes database contents.

1. Create the database through the approved database provider and add its private connection address to `.env.local` as `DATABASE_URL`.
2. From the project folder, run:

   ```powershell
   node scripts/apply-schema.mjs
   ```

   This script creates the main ComRes tables and applies the included schema updates. It is not run automatically when the website starts, when tests run, or when a production build is created. Do not run it against a shared or live database without explicit approval and a suitable backup.
3. Set `AUTH_SECRET` before starting the website. Add Stripe or image-storage settings only if you will test those services.

The payment-specific database update is separate from fresh database setup. `scripts/apply-payment-migration.mjs` has checks for a designated target and is **not** a general setup command for a new database. Treat it as a separately authorized maintenance action.

Deposits use the current booking-payment process. The old endpoint for directly charging a deposit after a bad hire is disabled and returns HTTP 410. Do not treat the old direct-charge procedure as a supported payment workflow.

### First administrator account

Public registration supports requester and organisation accounts, not administrator accounts. The schema setup creates database tables but does not create an administrator. This package does not provide a supported first-administrator setup procedure. An authorized administrator or database operator must arrange that account through an approved process outside this package.

The source handover excludes the sample-data seed utility because it contains default demo credentials. It also excludes the database reset utility because it can remove account, equipment, request, booking, and blackout data. Do not restore or use seed/reset tools on a shared or live database.

Before any other database operation, confirm the target, review the exact script and its safety checks, and make an appropriate backup.

## Start the website

After dependencies, private settings, and the authorized database are ready:

```powershell
npm run dev
```

Open [the local ComRes website](http://localhost:3000). Pages that use accounts, equipment, or external services need the corresponding database and service settings.

## Run checks and create a production build

Available project checks:

```powershell
node --test
npm run lint
npm run build
npm run start
```

The first command runs the automated test files. The others check code style, create a production build, and start that build. These instructions do not mean the commands were run as part of this document review. The handover package does not contain a separate, verified test report.

## Find the main project areas

| Project area | What it contains |
| --- | --- |
| `app/` | Website pages and the behind-the-scenes routes used by the website. |
| `components/` | Shared page elements, such as navigation and equipment cards. |
| `lib/` | Shared rules for sign-in, availability, requests, payments, and equipment handover. |
| `db/schema.sql` | The database structure used by ComRes. |
| `scripts/` | Database setup, migration, and verification tools. Review each tool before use. |
| `public/` | Images and other files shown by the website. |
| `docs/superpowers/` | Technical design and implementation history. Some documents describe earlier decisions; read any status notice at the top. |
| `.env.example` | Example settings with placeholders only. |
| `ComRes_Final_Handover.zip` | Source, tests, assets, setup material, and selected technical documents. |

The source ZIP excludes private environment files, Git history, downloaded packages, build output, AI-tool instruction files, and seed/reset utilities. The organisation-account implementation plan retains its academic AI-use disclosure. The AI-assisted plans in the archive are historical project records, not user instructions or proof that planned work or testing was completed.

## Client documents available here

- [Client Handover Start Page](./ComRes_Client_Handover/00_START_HERE.md) — client-document navigation and missing-materials list.
- [Project Overview](./ComRes_Client_Handover/01_Project_Overview/Project_Overview_and_Evaluation.pdf)
- [Editable Project Overview](./ComRes_Client_Handover/01_Project_Overview/Project_Overview_and_Evaluation.md)
- [Deliverables and Scope Guide](./ComRes_Client_Handover/01_Project_Overview/ComRes_Deliverables_and_Scope_Guide_Final.pdf)
- [Editable Deliverables and Scope Guide](./ComRes_Client_Handover/01_Project_Overview/ComRes_Deliverables_and_Scope_Guide_Final.docx)
- [Markdown Deliverables and Scope Guide](./ComRes_Client_Handover/01_Project_Overview/ComRes_Deliverables_and_Scope_Guide_Final.md)

The separate requester, provider, and administrator guides; detailed testing reports; presentation materials; and signed client-acceptance evidence are not present in this personal repository package. Do not treat their absence as evidence that testing or acceptance took place.

Copies labelled as backups are retained for project history and are not current client deliverables. Use the Project Overview and the files named Deliverables and Scope Guide above for the current handover.
