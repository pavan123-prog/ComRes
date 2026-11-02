# ComRes

ComRes is a community equipment-sharing application for Community Resource Network SA. It helps people find equipment and request a paid hire, a temporary free loan, or a permanent donation.

## Who uses ComRes?

- **Requesters** create an account, browse equipment, check availability, submit requests, and follow or cancel eligible requests.
- **Organisation providers** apply for an account. An administrator must approve the organisation before it can manage its own listings and requests.
- **Administrators** review organisation applications, manage equipment and requests, and oversee handover, returns, and eligible payment actions.

Features described here are present in the project code. That does not mean every user journey has been tested with real users or on a live service. The handover materials identify what has and has not been verified.

## Project documents

- [Client Handover Start Page](./ComRes_Client_Handover/00_START_HERE.md) — navigation to the current client documents and a clear list of missing evidence.
- [Project Overview](./ComRes_Client_Handover/01_Project_Overview/Project_Overview_and_Evaluation.pdf) — a plain-language introduction and evaluation summary.
- [Editable Project Overview](./ComRes_Client_Handover/01_Project_Overview/Project_Overview_and_Evaluation.md) — the Markdown source for the overview PDF.
- [Deliverables and Scope Guide](./ComRes_Client_Handover/01_Project_Overview/ComRes_Deliverables_and_Scope_Guide_Final.pdf) — what the project includes, its limits, and the evidence still needed.
- [Editable Deliverables and Scope Guide](./ComRes_Client_Handover/01_Project_Overview/ComRes_Deliverables_and_Scope_Guide_Final.docx) — the Word version of the guide.
- [Markdown Deliverables and Scope Guide](./ComRes_Client_Handover/01_Project_Overview/ComRes_Deliverables_and_Scope_Guide_Final.md) — the plain-text editable source.
- [Developer Setup and Technical Handover](./HANDOVER_README.md) — installation, private settings, database preparation, testing, and maintenance.

The individual requester, provider, and administrator guides, as well as separate test reports and presentation evidence, are not included in this personal repository handover.

Copies labelled as backups are retained for project history. Use the files named Project Overview and Deliverables and Scope Guide above as the current client documents.

## Technology in brief

The application uses Next.js and React to display the website, PostgreSQL to store information, Stripe for card setup and booking-payment actions, and Vercel Blob for uploaded images. These names are included for developers; users do not need to know them to understand the ComRes workflows.

## Run the application

Developers need Node.js 20.9 or later, npm, an authorized PostgreSQL database prepared with the project schema, and service credentials for any external features they want to try.

From the project folder:

```powershell
npm ci
Copy-Item .env.example .env.local
```

Add private settings to `.env.local` as described in the [technical handover](./HANDOVER_README.md). Do not share that file. Database setup is a separate, authorized step; starting the website does not prepare a database automatically.

Start the local development website:

```powershell
npm run dev
```

Then open [the local ComRes website](http://localhost:3000).

## Check the project

The following commands are available from the project folder:

```powershell
node --test
npm run lint
npm run build
```

These are instructions, not a statement that the commands were run for this handover review. See the [Deliverables and Scope Guide](./ComRes_Client_Handover/01_Project_Overview/ComRes_Deliverables_and_Scope_Guide_Final.pdf) for current evidence and outstanding checks.
