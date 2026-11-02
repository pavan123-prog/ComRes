# ComRes Deliverables and Scope Guide

**Purpose:** A plain-language guide to what ComRes includes, how the main user journeys work, and what still needs to be checked.

## 1. Project purpose

ComRes is a community equipment-sharing application for Community Resource Network SA. It helps people discover equipment and request a paid hire, a temporary free loan, or a permanent donation.

The project aims to make equipment easier to find and request, give approved community organisations a way to manage their own listings, and give administrators tools to oversee applications, equipment, requests, handovers, and relevant payments.

This guide reflects features found in the personal repository's application code and the documents available in this handover. A feature in code is not, by itself, proof that the complete journey has been tested or accepted.

## 2. Roles and what each can do

### Requesters

Requesters can register or sign in, browse equipment, check dates and quantity, submit requests, view request status, and cancel eligible requests. They can provide feedback or reviews where those features are available.

### Organisation providers

An organisation applies for an account and waits for an administrator's decision. After approval, it can create and edit its own equipment listings, manage supported listing details and images, mark listings unavailable for selected periods, retire or restore its listings, and respond to requests for its equipment.

### Administrators

Administrators review organisation applications, manage equipment and unavailable periods, review requests, and oversee equipment handover, returns, payment status, and eligible refund actions. The dashboard provides operational summaries; it is not an exportable analytics service.

Public registration does not create administrator accounts. This package does not include a first-administrator setup procedure.

## 3. Main request journeys

1. A requester selects equipment, dates, and quantity, then submits a request.
2. ComRes checks the request against the listed quantity, existing bookings, and unavailable periods.
3. The equipment's provider or an administrator reviews the request.
4. If approved, the requester follows the booking status and completes card setup when required.
5. The responsible provider or administrator records the collection, return condition, or donation handover.

The application supports three request types:

- **Paid hire:** the requester pays any configured hire fee and deposit through the current booking-payment process.
- **Temporary free loan:** no hire fee is charged, although a deposit may be configured.
- **Permanent donation:** after the handover is recorded, the donation is complete and the listed quantity is reduced.

Pending or approved requests may be cancelled by an authorized participant while cancellation is allowed. A request cannot be treated as an ordinary cancellation after collection or a completed donation handover. Relevant successful payments may enter a refund process.

## 4. Payment behavior and limits

After approval, the requester can use Stripe's hosted page to save a card. Stripe sends a signed update to the application, which confirms the card setup and processes configured booking payments. The application records payment status rather than relying only on the browser's return page.

Some successful deposits can be refunded automatically after an eligible return. Other return conditions need an authorized administrator to review them.

Deposits are part of the current booking-payment process. The old option for directly charging a deposit after a bad hire has been disabled. ComRes does not implement provider payouts. This handover contains no evidence of a Stripe payment demonstration.

Use Stripe test mode for any future demonstration. Do not use real payment details or make a live charge for presentation purposes.

## 5. Included features and items not included

### Included in the application code

- Equipment catalogue, item information, availability, and request submission.
- Requester accounts, request history, eligible cancellations, notifications, feedback, and reviews.
- Organisation applications and administrator review.
- Approved provider listing management, including editing and retire/restore actions.
- Administrator equipment, request, organisation, equipment-handover, and payment-related tools.
- Records of equipment collection, return condition, and donation handover.
- Database schema, setup utilities, automated test files, and selected technical records.

### Not included or not established by this handover

- Public administrator sign-up or a supported in-package process for creating the first administrator.
- Provider payouts or marketplace settlement through Stripe.
- Direct bad-hire deposit charging.
- Exportable analytics or reporting beyond the administrator's operational summaries.
- A separate verified test report, full manual test results, usability study, security assessment, or signed client acceptance.
- Separate requester, provider, and administrator user guides, presentation screenshots, or a presentation runbook in this personal repository package.
- System or database diagrams. None were found in the project materials reviewed for this personal handover.

The project includes database setup and maintenance utilities. They are not run when the website starts. Database setup changes data and must only be performed by an authorized operator against the intended database. The source archive excludes the sample-data and reset utilities; the [technical handover](https://github.com/pavan123-prog/ComRes/blob/main/HANDOVER_README.md) explains this limitation.

## 6. Evaluation checklist

**How to read the status:** “Present in code” means the feature exists in the application, not that the complete journey has been tested. “Partly evidenced” means some related code or test reference exists, but a full demonstration is still needed. “Not tested” or “not verified” means this handover has no recorded result for that check.

| Evaluation area | What is present | Status and next evidence needed |
| --- | --- | --- |
| 1. Scope completion | The main requester, provider, administrator, equipment, and booking workflows are represented in code. | **Partly evidenced.** Walk through the agreed scope on an authorized deployment. |
| 2. Availability and booking | Date and quantity checks consider bookings and unavailable periods. | **Partly evidenced.** Record a browser test for available and conflicting dates/quantities. |
| 3. Listing management | Approved providers can manage their own listings; administrators can manage inventory. | **Present in code; manual check outstanding.** Test approval, create/edit, images, unavailable dates, retire/restore, and moderation. |
| 4. Transaction workflows | Paid hire, free loan, donation, approval, and equipment handover records are supported. | **Partly evidenced.** Complete one end-to-end example of each request type. |
| 5. Cancellation and returns | Eligible cancellation and collection/return recording are supported. | **Partly evidenced.** Record cancellation, return condition, availability, and any refund outcome. |
| 6. Payment demonstration | Card setup, booking-payment records, payment updates, and eligible refund actions are represented in code. | **Not demonstrated here.** Demonstrate a Stripe test-mode payment, confirmation, and eligible refund. |
| 7. Administrative reporting | The administrator dashboard provides operational summaries and management views. | **Not tested here.** Check role access and displayed summaries. No export/report feature is claimed. |
| 8. Usability | User-facing pages and role-based tools are present. | **Not tested here.** Observe users completing common tasks and review mobile layouts. |
| 9. Database quality | A database schema and setup utilities are included. | **Not verified on a live test database.** Check relationships, rules, duplicate prevention, and safe setup. |
| 10. Security and privacy | Sign-in and role-based access controls are present in the application. | **Not a security assessment.** Review access control and personal-data handling before release. |
| 11. Client acceptance | No signed acceptance record is included. | **Not recorded here.** Obtain dated client review and approval against agreed criteria. |

Automated test files are included in the source archive. A separate test report and independently verified test-run results are not included in this handover. Code or test files alone do not prove a feature has passed a complete user journey.

## 7. Documents available

- [Project Overview](./Project_Overview_and_Evaluation.pdf) — introduction and evaluation summary.
- [Developer Setup and Technical Handover](https://github.com/pavan123-prog/ComRes/blob/main/HANDOVER_README.md) — installation, private settings, database preparation, testing, and maintenance.
- [ComRes source archive](https://github.com/pavan123-prog/ComRes/blob/main/ComRes_Final_Handover.zip) — application source, assets, automated test files, and selected technical records.

The separate requester, provider, and administrator guides; detailed testing and presentation evidence; and signed client-acceptance materials are not in this personal repository handover. This guide does not link to documents that are not present.

## 8. Technical terms, in everyday language

- **Database:** the protected place where ComRes keeps accounts, equipment, and request information.
- **Unavailable period:** dates when an item cannot be requested, for example because it is already reserved or the provider has blocked the dates.
- **Webhook:** a secure message from Stripe telling ComRes that a card or payment event has changed.
- **Migration:** a controlled change to the database structure. It is separate from starting the website and must be run only against an authorized target.
- **Automated test:** a programmed check of a specific rule. It does not replace a person completing and reviewing the whole user journey.

## 9. Academic AI-use and contribution record

The source archive retains the disclosure in the historical organisation-account implementation plan:

> “This implementation plan was developed with assistance from Claude Sonnet 5. The example commit messages below omit tool-specific co-author trailers; actual commit authorship remains in the Git history.”

The related plan's unchecked tasks are historical records, not evidence that the work or tests were completed. This guide does not change application code, Git history, commit authorship, or contribution records.
