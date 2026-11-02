# Project Overview and Evaluation

## Project overview

ComRes is a community equipment-sharing application for Community Resource Network SA. It helps people find equipment and request a paid hire, a temporary free loan, or a permanent donation.

The project code contains the workflows described below. Code being present does not prove that a complete journey has been tested with real users or on a live service. This handover does not claim that usability testing, a full security assessment, or client acceptance has taken place.

## Who uses ComRes?

- **Requesters** can register, browse equipment, check dates and quantities, send a request, follow its status, and cancel eligible requests.
- **Organisation providers** apply for an account. An administrator must approve the application before the organisation can manage its own listings and respond to requests for those items.
- **Administrators** review organisation applications, manage equipment and requests, and oversee equipment handover, returns, and eligible payment actions.

Public registration does not create administrator accounts. The first administrator must be arranged through an authorized process outside this package.

## How requests work

1. A requester chooses an item, dates, and quantity, then sends a request.
2. ComRes checks whether the item is available for those dates and the requested quantity.
3. The item's provider or an administrator reviews the request.
4. If approved, the requester can follow the booking and complete card setup when payment is required.
5. The responsible provider or administrator records collection, return condition, or donation handover as applicable.

Eligible requests can be cancelled before collection or a completed donation handover. A completed donation is permanent and reduces the available listed quantity.

## Payments in plain language

For an approved request, the requester can use Stripe's hosted card setup. The current booking-payment process records relevant hire fees and deposits and confirms payment changes through Stripe updates. Certain completed returns can qualify for an automatic deposit refund; other return conditions need an authorized administrator to review them.

Deposits are handled through the current booking-payment process. The old option for directly charging a deposit after a bad hire has been disabled. ComRes does not provide provider payouts. This handover does not include evidence of a test or live payment demonstration.

## What is included

- An equipment catalogue, item information, availability checks, and request submission.
- Requester accounts, request history, eligible cancellations, notifications, feedback, and reviews.
- Organisation applications and, after approval, tools to create, edit, retire, and restore the organisation's own listings.
- Administrator tools for organisation review, equipment, requests, operational summaries, equipment handover records, and eligible payment actions.
- Project source, database schema and setup instructions, automated test files, and technical documents in the source archive.

The project includes operational dashboard summaries. It does not include an exportable analytics or reporting service.

## Evaluation at a glance

**How to read the status:** “In code” means the feature is present in the application, not that the full user journey has been tested. “Partly evidenced” means some related code or test reference exists, but a complete demonstration is still needed. “Not tested” or “not verified” means this handover contains no recorded result for that check.

| Area | What the project code supports | Evidence status and remaining check |
| --- | --- | --- |
| Scope completion | Requester, provider, administrator, equipment, and booking workflows are represented in the source. | **Partly evidenced.** Walk through the agreed scope on an authorized deployment. |
| Availability and booking | Checks dates, quantities, existing bookings, and unavailable periods before a request is made. | **Partly evidenced.** Record a complete browser journey, including unavailable dates and quantities. |
| Listing management | Approved providers can manage their own listings; administrators can manage inventory. | **Implemented in code; not manually verified here.** Test approval, editing, images, unavailable periods, and retire/restore actions. |
| Transaction workflows | Supports paid hire, free loan, and permanent donation requests, approvals, and equipment handover records. | **Partly evidenced.** Complete one end-to-end journey for each request type. |
| Cancellation and returns | Supports eligible cancellations and records collection and return condition. | **Partly evidenced.** Verify cancellation, return, availability, and refund outcomes in a controlled test. |
| Payment demonstration | Includes card setup, booking-payment records, payment updates, and eligible refund actions. | **Not demonstrated here.** Use Stripe test mode to demonstrate payment, confirmation, and an eligible refund. |
| Administrative reporting | Provides operational summaries and administrator views. | **Not tested here.** Check access and displayed counts. No exportable reporting feature is claimed. |
| Usability | User-facing pages and role-specific tools are present. | **Not tested here.** Collect user feedback and review common tasks on desktop and mobile. |
| Database quality | A database schema and setup utilities are included. | **Not verified against a live test database.** Check relationships, rules, duplicate prevention, and safe setup. |
| Security and privacy | Sign-in, role checks, and privacy-related application code are present. | **Not a security assessment.** Review access controls and personal-data handling before release. |
| Client acceptance | No signed acceptance record is included in this package. | **Not recorded here.** Obtain dated client review and approval against agreed criteria. |

Automated test files are included in the source archive, but a separate test report and verified test-run results are not included in this personal handover. Do not interpret the presence of tests as proof that the full application has passed testing.

## Documents and evidence in this handover

- [Deliverables and Scope Guide](./ComRes_Deliverables_and_Scope_Guide_Final.pdf) — project scope, user roles, limits, and evaluation status.
- [Editable Deliverables and Scope Guide](./ComRes_Deliverables_and_Scope_Guide_Final.docx) — editable Word copy.
- [Developer Setup and Technical Handover](https://github.com/pavan123-prog/ComRes/blob/main/HANDOVER_README.md) — setup, private settings, database preparation, and maintenance.
- [ComRes source archive](https://github.com/pavan123-prog/ComRes/blob/main/ComRes_Final_Handover.zip) — source code, tests, assets, and selected technical records.

The requester, provider, and administrator user guides; a standalone testing report; presentation screenshots/runbook; and signed client-acceptance evidence are not included in this personal repository handover. No links are provided for those missing documents.

## Technical background

The application is built with Next.js and React. It stores information in PostgreSQL, uses Stripe for card setup and booking-payment actions, and uses Vercel Blob for uploaded images. In everyday terms, these services provide the website, its database, card-payment processing, and image storage.

Database preparation is a separate developer/operator task. The website does not create or update the database automatically when it starts. The [technical handover](https://github.com/pavan123-prog/ComRes/blob/main/HANDOVER_README.md) explains the authorized setup steps and safety limits.
