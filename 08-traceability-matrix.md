# Traceability Matrix and Coverage Check

**Author** Brandon Narine  
**Date** November 24, 2026  
**Status** Phase 1 business analysis. This matrix traces the Release 1 requirement set as designed, before any build.

Forward traceability checks whether every business requirement has a story, a specification, a designed test and a planned user acceptance scenario. Backward traceability checks that every test case leads through a specified requirement to a business need, and that every specified requirement has a business reason.

## Traceability matrix

The US column uses the story-to-BR links in Artifact 4. The FR and SEC columns use the explicit BR and story links in Artifact 5. Parentheses in the TC column give the requirements that link each case to this row and the story IDs cited by the case. A case with several requirement links can appear in several rows. [Business Requirements Document](01-business-requirements-document.md) · [User Stories](04-user-stories-and-acceptance-criteria.md) · [Functional Specification](05-functional-specification.md) · [Test Cases](06-test-cases.md)

The UAT column uses the BR links in Artifact 7 and the scenarios reached through the row's linked stories. Parentheses show the direct BR link where present and the shared story IDs. SEC IDs in that column identify business checks visible in the scenario steps or success criteria. An individual check can cover part of a security requirement, such as staff keeping card data out of free text. System test cases cover the technical controls. [UAT scenarios](07-uat-plan.md#uat-scenarios)

| BR ID and short title | US IDs | FR IDs | SEC IDs | TC IDs | UAT IDs |
| --- | --- | --- | --- | --- | --- |
| BR-01 Product catalogue | US-01, US-02 | FR-01, FR-02, FR-03 (US-01)<br>FR-04, FR-05 (US-02) | SEC-15 (US-01, US-03, US-04, US-07, US-13, US-23) | TC-01 (FR-01, US-01)<br>TC-02 (FR-02, US-01)<br>TC-03 (FR-03, US-01)<br>TC-04 (FR-04, FR-05, US-02)<br>TC-60 (SEC-15, US-01, US-03, US-04, US-07, US-13, US-23) | UAT-05 (BR-01, US-01, US-02) |
| BR-02 Stock receiving | US-03 | FR-06, FR-07, FR-08 (US-03) | SEC-15 (US-01, US-03, US-04, US-07, US-13, US-23) | TC-05 (FR-06, US-03)<br>TC-06 (FR-06, FR-07, US-03)<br>TC-07 (FR-06, FR-08, US-03)<br>TC-60 (SEC-15, US-01, US-03, US-04, US-07, US-13, US-23) | UAT-01, UAT-04 (BR-02, US-03) |
| BR-03 Stock adjustment reasons | US-04 | FR-09, FR-13 (US-04)<br>FR-10 (US-04, US-05) | SEC-15 (US-01, US-03, US-04, US-07, US-13, US-23) | TC-08 (FR-09, US-04)<br>TC-09, TC-13 (FR-10, US-04, US-05)<br>TC-11 (FR-13, US-04)<br>TC-12 (FR-13, US-04, US-05)<br>TC-60 (SEC-15, US-01, US-03, US-04, US-07, US-13, US-23) | UAT-02, UAT-03 (BR-03, US-04) |
| BR-04 Customer records and balances | US-06, US-07, US-14 | FR-14, FR-15 (US-06)<br>FR-16, FR-18 (US-07)<br>FR-33 (US-14) | SEC-16 (US-06)<br>SEC-17 (US-06, US-13) | TC-14 (FR-14, US-06)<br>TC-15 (FR-15, US-06)<br>TC-16 (FR-16, FR-18, US-07, US-08, US-10)<br>TC-17 (FR-16, US-07, US-08)<br>TC-27 (FR-33, US-14)<br>TC-28, TC-29 (FR-33, US-13, US-14)<br>TC-61 (SEC-16, US-06)<br>TC-62 (SEC-17, US-06, US-13) | UAT-01 (BR-04, US-07, US-14, SEC-17)<br>UAT-06 (US-14)<br>UAT-07 (BR-04, US-06, US-07, SEC-16, SEC-17) |
| BR-05 Sale creation and pricing | US-07, US-08 | FR-16, FR-17, FR-18 (US-07)<br>FR-19, FR-20 (US-08)<br>FR-21 (US-08, US-11) | SEC-15 (US-01, US-03, US-04, US-07, US-13, US-23) | TC-16 (FR-16, FR-18, FR-19, US-07, US-08, US-10)<br>TC-17 (FR-16, FR-19, US-07, US-08)<br>TC-18 (FR-17, US-07)<br>TC-19 (FR-20, US-08)<br>TC-20 (FR-21, US-08, US-11)<br>TC-60 (SEC-15, US-01, US-03, US-04, US-07, US-13, US-23) | UAT-01 (BR-05, US-07, US-08)<br>UAT-05 (BR-05, US-08)<br>UAT-07 (BR-05, US-07) |
| BR-06 Stock availability | US-09 | FR-22 (US-09) | SEC-21 (US-09, US-10) | TC-21, TC-22 (FR-22, US-09, US-10)<br>TC-66 (SEC-21, US-09, US-10) | UAT-01 (BR-06, US-09, SEC-21) |
| BR-07 Stock deduction at sale completion | US-10 | FR-23 (US-10) | SEC-21 (US-09, US-10) | TC-16 (FR-23, US-07, US-08, US-10)<br>TC-21, TC-22 (FR-23, US-09, US-10)<br>TC-66 (SEC-21, US-09, US-10) | UAT-01 (BR-07, US-10, SEC-21) |
| BR-08 Numbered printable invoices | US-11, US-12 | FR-21 (US-08, US-11)<br>FR-24, FR-25, FR-27 (US-11)<br>FR-26 (US-11, US-12)<br>FR-28 (US-12, US-15) | SEC-18 (US-11) | TC-20 (FR-21, US-08, US-11)<br>TC-23 (FR-24, FR-25, US-11)<br>TC-24 (FR-24, FR-26, US-11, US-12)<br>TC-25 (FR-24, US-11)<br>TC-26 (FR-27, FR-28, US-11, US-12, US-15)<br>TC-63 (SEC-18, US-11) | UAT-01, UAT-05 (BR-08, US-11, US-12, SEC-18)<br>UAT-07 (BR-08, US-11) |
| BR-09 Full and partial payments | US-13, US-15 | FR-29, FR-30 (US-13)<br>FR-34, FR-35 (US-15) | SEC-15 (US-01, US-03, US-04, US-07, US-13, US-23)<br>SEC-17 (US-06, US-13) | TC-28, TC-29 (FR-29, US-13, US-14)<br>TC-30, TC-31 (FR-30, US-13)<br>TC-32 (FR-29, FR-30, US-13, US-14)<br>TC-33 (FR-34, FR-35, US-15)<br>TC-60 (SEC-15, US-01, US-03, US-04, US-07, US-13, US-23)<br>TC-62 (SEC-17, US-06, US-13) | UAT-01 (BR-09, US-13, US-15, SEC-17)<br>UAT-06 (BR-09, US-13, US-15) |
| BR-10 Payment status, balances and history | US-12, US-14, US-15 | FR-28 (US-12, US-15)<br>FR-31, FR-32, FR-33 (US-14)<br>FR-34, FR-35 (US-15) | None | TC-26 (FR-28, US-11, US-12, US-15)<br>TC-27 (FR-31, FR-32, FR-33, US-14)<br>TC-28, TC-29 (FR-31, FR-32, FR-33, US-13, US-14)<br>TC-32 (FR-31, FR-32, US-13, US-14)<br>TC-33 (FR-34, FR-35, US-15) | UAT-01 (BR-10, US-12, US-14, US-15)<br>UAT-05 (US-12)<br>UAT-06 (BR-10, US-14, US-15) |
| BR-11 Receivables | US-16 | FR-36 (US-16) | None | TC-34 (FR-36, US-16) | UAT-01, UAT-06 (BR-11, US-16) |
| BR-12 Low Stock List | US-17 | FR-37 (US-17) | None | TC-35 (FR-37, US-17) | UAT-01, UAT-06 (BR-12, US-17) |
| BR-13 Sales reporting | US-18 | FR-38, FR-39 (US-18) | None | TC-36 (FR-38, US-18)<br>TC-37 (FR-39, US-18) | UAT-06 (BR-13, US-18) |
| BR-14 Stock on hand valuation | US-19 | FR-40 (US-19) | None | TC-38 (FR-40, US-19) | UAT-06 (BR-14, US-19) |
| BR-15 Staff roles and permissions | US-20, US-21 | None | SEC-01, SEC-03 (US-20, US-21)<br>SEC-02, SEC-04 (US-21)<br>SEC-05 (US-20, US-22)<br>SEC-06 (US-02, US-20, US-23)<br>SEC-07, SEC-08, SEC-09, SEC-11 (US-20)<br>SEC-13 (US-22) | TC-43 (SEC-01, US-20, US-21)<br>TC-44 (SEC-02, US-21)<br>TC-45 (SEC-03, US-20, US-21)<br>TC-46 (SEC-04, US-21)<br>TC-47 (SEC-05, US-20, US-22)<br>TC-48 (SEC-06, US-02, US-20)<br>TC-49 (SEC-06, US-20, US-23)<br>TC-50 (SEC-07, US-20)<br>TC-51 (SEC-08, US-20)<br>TC-52 (SEC-09, US-20)<br>TC-55 (SEC-11, US-20)<br>TC-58 (SEC-13, US-22) | UAT-08 (BR-15, US-20, US-21, SEC-01, SEC-02, SEC-06, SEC-13) |
| BR-16 Individual staff attribution | US-21, US-22 | None | SEC-01, SEC-03 (US-20, US-21)<br>SEC-02, SEC-04 (US-21)<br>SEC-12 (US-02, US-04, US-05, US-13, US-21, US-22) | TC-43 (SEC-01, US-20, US-21)<br>TC-44 (SEC-02, US-21)<br>TC-45 (SEC-03, US-20, US-21)<br>TC-46 (SEC-04, US-21)<br>TC-56 (SEC-12, US-02, US-04, US-05, US-13, US-22)<br>TC-57 (SEC-12, US-21, US-22) | UAT-02, UAT-03, UAT-05, UAT-06 (BR-16, US-22, SEC-12)<br>UAT-08 (BR-16, US-21, US-22, SEC-01, SEC-02, SEC-12) |
| BR-17 Price approval and oversight | US-02, US-08 | FR-04, FR-05 (US-02)<br>FR-19, FR-20 (US-08)<br>FR-21 (US-08, US-11) | SEC-06 (US-02, US-20, US-23)<br>SEC-10 (US-02, US-05) | TC-04 (FR-04, FR-05, US-02)<br>TC-16 (FR-19, US-07, US-08, US-10)<br>TC-17 (FR-19, US-07, US-08)<br>TC-19 (FR-20, US-08)<br>TC-20 (FR-21, US-08, US-11)<br>TC-48 (SEC-06, US-02, US-20)<br>TC-49 (SEC-06, US-20, US-23)<br>TC-53, TC-54 (SEC-10, US-02, US-05) | UAT-01 (BR-17, US-08)<br>UAT-05 (BR-17, US-02, US-08, SEC-10)<br>UAT-08 (SEC-06) |
| BR-18 Higher value adjustment approval | US-04, US-05 | FR-10 (US-04, US-05)<br>FR-11, FR-12 (US-05) | SEC-10 (US-02, US-05) | TC-09 (FR-10, US-04, US-05)<br>TC-10 (FR-11, FR-12, US-05)<br>TC-12 (FR-11, FR-12, US-04, US-05)<br>TC-13 (FR-10, FR-11, US-04, US-05)<br>TC-53, TC-54 (SEC-10, US-02, US-05) | UAT-02 (US-04)<br>UAT-03 (BR-18, US-04, US-05, SEC-10) |
| BR-19 Protected audit trail | US-02, US-04, US-05, US-13, US-22 | None | SEC-05 (US-20, US-22)<br>SEC-10 (US-02, US-05)<br>SEC-12 (US-02, US-04, US-05, US-13, US-21, US-22)<br>SEC-13, SEC-14 (US-22) | TC-47 (SEC-05, US-20, US-22)<br>TC-53, TC-54 (SEC-10, US-02, US-05)<br>TC-56 (SEC-12, US-02, US-04, US-05, US-13, US-22)<br>TC-57 (SEC-12, US-21, US-22)<br>TC-58 (SEC-13, US-22)<br>TC-59 (SEC-14, US-22) | UAT-01 (US-13)<br>UAT-02 (BR-19, US-04, US-22, SEC-12)<br>UAT-03 (BR-19, US-04, US-05, US-22, SEC-10, SEC-12)<br>UAT-05 (BR-19, US-02, US-22, SEC-10, SEC-12)<br>UAT-06 (BR-19, US-13, US-22, SEC-12)<br>UAT-08 (BR-19, US-22, SEC-12, SEC-13, SEC-14) |
| BR-20 Authorized search and filtering | US-23 | FR-41, FR-42, FR-43, FR-44 (US-23) | SEC-06 (US-02, US-20, US-23)<br>SEC-15 (US-01, US-03, US-04, US-07, US-13, US-23) | TC-39 (FR-41, US-23)<br>TC-40 (FR-42, US-23)<br>TC-41 (FR-43, US-23)<br>TC-42 (FR-44, US-23)<br>TC-48 (SEC-06, US-02, US-20)<br>TC-49 (SEC-06, US-20, US-23)<br>TC-60 (SEC-15, US-01, US-03, US-04, US-07, US-13, US-23) | UAT-01, UAT-04, UAT-07 (BR-20, US-23)<br>UAT-08 (BR-20, US-23, SEC-06) |
| BR-21 Backup and restore | US-24 | None | SEC-19, SEC-20 (US-24) | TC-64 (SEC-19, US-24)<br>TC-65 (SEC-20, US-24) | UAT-09 (BR-21, US-24, SEC-19, SEC-20) |

## Coverage summary

| Coverage measure | Total | With coverage |
| --- | ---: | ---: |
| BRs with at least one US | 21 | 21 |
| BRs with at least one FR or SEC | 21 | 21 |
| BRs with at least one TC | 21 | 21 |
| BRs with at least one UAT scenario | 21 | 21 |
| US with at least one implementing FR or SEC | 24 | 24 |
| US with at least one TC | 24 | 24 |
| FRs with at least one TC | 44 | 44 |
| SECs with at least one TC | 21 | 21 |
| TCs with at least one FR or SEC link | 66 | 66 |
| UAT scenarios with at least one US or BR link | 9 | 9 |
| FRs with a UAT scenario through a linked US | 44 | 44 |

Count BRs by row and other IDs once per type across the matrix. Count coverage using the IDs shown for each link.

## Orphans and gaps

- **BRs with no downstream coverage at any stage.** None found.
- **US with no FR or SEC.** None found.
- **FRs or SECs with no TC.** None found.
- **TCs with no requirement link.** None found.

UAT-08 covers role access. The designed system tests cover the minimum password boundary (SEC-03), lockout (SEC-05), signed-out direct access (SEC-07), idle expiry (SEC-09) and protected access after sign-out (SEC-11). These checks have no dedicated expected outcome in Artifact 7. UAT-01 and UAT-07 cover the SEC-17 staff practice. TC-62 covers storage verification and depends on the free-text handling contract. FR-40 has a designed test and a UAT scenario. Numeric reconciliation in both depends on the BR-14 decision and its required inputs.

## Consolidated open items register

| Open item | Raised in | Decision or confirmation needed | Sits with |
| --- | --- | --- | --- |
| Wholesale pricing policy | Artifact 2, Artifact 4 US-08 | Confirm whether one approved wholesale price per Product applies to all wholesale Customers or whether negotiated Customer pricing is needed. | Owner and Manager at scope sign off |
| Invoice and Payment retention | Artifact 2, Artifact 4 US-15, Artifact 5 rule 4 | Confirm the retention period and applicable obligations. The draft retains both record types throughout Release 1. | Owner and Manager at scope sign off |
| Sales with unpaid balances | Artifact 2, Artifact 4 US-07, Artifact 5 rule 2 | Confirm the default of displaying the existing balance and allowing an otherwise valid sale. | Owner and Manager at scope sign off |
| Adjustment approval authority and threshold | Artifact 2, Artifact 4 US-05, Artifact 5 rules 8 and 9 | Confirm the approver and threshold. The draft uses owner approval above a fictional G$10,000 and an independent approver when a second authorized approver is available. | Owner and Manager at scope sign off |
| Invoice tax display | Artifact 2, Artifact 4 US-11, Artifact 5 rule 4 | Confirm whether a separate tax line is required and what approved tax name, rate and amount to display. | Owner and Manager at scope sign off |
| Invoice delivery | Artifact 2, Artifact 4 US-12, Artifact 5 rule 4 | Confirm whether printed delivery meets the need or emailed delivery should be considered through scope change. | Owner and Manager at scope sign off |
| Separate Payment receipt | Artifact 2, BRD, Task 1 through Task 5 and Task 7 reports | Confirm whether each Payment needs a separate receipt. The baseline uses the updated printed Invoice and Payment history. | Owner and Manager at scope sign off |
| Stock on hand valuation | BRD BR-14, Artifact 4 US-19, FR-40, TC-38, UAT-06 | Confirm the reporting basis and the inputs it requires so expected totals can be calculated and reconciled. This decision is separate from the rule 8 adjustment calculation. | Owner and Manager at scope sign off |
| Staff permissions and owner oversight | Artifact 4 US-20, Artifact 5 permissions matrix, Task 6 and Task 7 reports | Confirm role and price-change permissions, including the distinction between the owner's Receivables List, Reports and Audit Trail View and the clerks' individual Customer, Invoice and stock receipt access. Confirm whether clerk-assisted record review meets the owner's oversight needs. | Owner and Manager at scope sign off |
| Wholesale price inference from adjustments | Task 6 and Task 7 reports, TC-09, TC-49 | Confirm whether Inventory Clerks may infer wholesale unit price from the signed quantity change and displayed adjustment value, given the Product List price restriction. | Owner and Manager at scope sign off |
| Inactive Products on Low Stock List | Task 6 and Task 7 reports, FR-37, TC-35 | Confirm whether inactive Products that meet the reorder threshold should appear or be suppressed. The current test includes them. | Owner and Manager at scope sign off |
| Held adjustment that can no longer post | Task 6 and Task 7 reports, FR-12, FR-13, TC-12 | Confirm the disposition and approval state of a held adjustment after a posting recheck blocks it. The blocked posting leaves stock unchanged. | Owner and Manager at scope sign off |
| Multiple Products on one Supplier Delivery | Artifact 3 ERD outline, Artifact 4 UC-03, Task 3 through Task 7 reports | Decide whether and how to group multiple Product receipts under one Supplier Delivery. The baseline describes per-Product receipts. | Owner and Manager at scope sign off |
| Free-text card-data handling | SEC-17, TC-62, Task 6 and Task 7 reports | Define what the system recognizes as card data in free text and how it handles a match across all stored fields and audit records. Confirm the synthetic verification input. | Build phase |
| Counter flow stock deduction timing | Artifact 3 F13, BRD BR-07, Artifact 4 UC-01, Artifact 5 rule 3, Task 3 through Task 7 reports | Reconcile the diagram's placement of deduction after payment with the specified deduction at sale completion, including completion without counter payment. | Owner and Manager at scope sign off |
| BRD baselines and invoice time target | BRD business objectives, Task 2 and Task 7 reports | Collect the current invoice preparation time, unavailable-stock sales count and unpaid-balance visibility and reconciliation measures. Confirm the baseline and post-release review periods and the numerical invoice time improvement target. | Owner and Manager at scope sign off |
| Overpayments | Artifact 4 US-13, Artifact 5 rule 5, Task 4 through Task 7 reports | Confirm business acceptance of the specified rejection of amounts above the current Invoice balance, with a corrected amount and no credit balance. | Owner and Manager at scope sign off |
| Receivables ordering | Artifact 4 US-16, Artifact 5 rule 6, Task 4 through Task 7 reports | Confirm business acceptance of the specified oldest-first ordering with ascending Invoice numbers for same-date ties. | Owner and Manager at scope sign off |
| Receipt mismatch confirmation | Artifact 4 UC-03, Artifact 5 rule 7, Task 4 through Task 7 reports | Confirm business acceptance of the specified hold and Inventory Clerk count confirmation before stock posts. | Owner and Manager at scope sign off |
| Sole approver and adjustment valuation | Artifact 4 US-02 and US-05, Artifact 5 rules 8 and 9, Task 4 through Task 7 reports | Confirm business acceptance of the specified audited self-approval when only one approver is authorized and the use of the approved wholesale price at submission for the adjustment threshold check. | Owner and Manager at scope sign off |
| Opening stock and Customer balances | Artifacts 1 and 2 dependencies, Task 5 report | Reconcile opening quantities against physical counts and opening balances against unpaid Invoices and recorded Payments before loading the opening records. | Build phase |
| Catalogue prices and reorder levels | Artifacts 1 and 2 dependencies, Artifact 4 US-17 | Confirm the approved prices and reorder levels used for sales and Low Stock List. | Owner and Manager at scope sign off |
| Inactivity timeout setting | Task 2 report, Artifact 5 SEC-09 | Confirm the Owner and Manager's operational inactivity setting. The Functional Specification sets the initial value at 15 minutes. | Owner and Manager at scope sign off |
| Approval and audit history data design | Artifact 3 ERD outline, Task 3 report, Artifact 5 audit trail requirements | Confirm how approval and audit history will be represented in the detailed data design using the specified actor, approval and event requirements. | Build phase |
| UAT schedule and readiness | Artifact 7 entry criteria and schedule outline, Task 7 report | Confirm the proposed six working days when entry criteria and participants are ready, and allow for fixes and retests based on findings. | Build phase |
| Project repository name | Spec Section 13, Task 1 and Task 2 reports | Confirm the working repository name `inventory-invoicing-system-ba`. | Owner and Manager at scope sign off |
| Fictional sample business | Spec Section 13, Task 1 and Task 2 reports | Confirm the electronics and accessories retail setting and the invented business name, if examples will name the store. | Owner and Manager at scope sign off |

## Change log

| Version | Date | Change |
| --- | --- | --- |
| 1 | November 24, 2026 | Drafted the Release 1 traceability matrix, coverage check and consolidated open items register. |
