# Implementation Roadmap

**Author** Brandon Narine  
**Date** November 24, 2026  
**Status** Phase 1 business analysis and planning is complete. Phases 2 through 6 are planned and have yet to start.

## Release 1 phase plan

| Phase | Purpose and planned activities | Inputs | Exit condition |
| --- | --- | --- | --- |
| 1. Business Analysis and Planning | Establish the business need, Release 1 scope and priorities, process models, stories, functional and security requirements, system tests, UAT plan, traceability and roadmap. | [BRD](01-business-requirements-document.md), [scope analysis](02-stakeholder-and-scope-analysis.md), [diagrams](03-diagrams/), [stories](04-user-stories-and-acceptance-criteria.md), [functional specification](05-functional-specification.md), [test cases](06-test-cases.md), [UAT plan](07-uat-plan.md) and [traceability matrix](08-traceability-matrix.md). | Analysis and planning set complete. Coverage verified against designed tests and planned UAT. Business decisions are tracked in the open items register. |
| 2. Design | A designer or developer designs screens and interfaces from the written screen specifications. Wireframes belong to this phase. Develop the data design from the ERD outline, including approval and audit history, and finalize the draw.io diagrams from Artifact 3. At the design gate the Owner and Manager confirms the BR-14 stock on hand valuation basis and inputs, and US-20 staff permissions and owner oversight. | [Diagram outlines](03-diagrams/diagram-outlines.md), [process flows](03-diagrams/process-flows-mermaid.md), Artifacts 4 and 5, and the [open items register](08-traceability-matrix.md#consolidated-open-items-register). | Screen, interface, data and diagram designs ready for build. Decisions needed to implement and verify the affected scope are recorded at the gate. |
| 3. Build | Implement the functional and security requirements. Start with accounts, roles and audit foundations, then build the catalogue, stock, sales, invoicing, payments and reporting in dependency order. Build audit capture and attribution into each affected workflow as it is implemented. Include daily backup and restore capability. | Artifacts 1, 2, 4, 5 and 8, and the Phase 2 designs and decisions. | Release 1 workflows, controls and backup and restore capability ready for system testing against the specified requirements. |
| 4. System Testing | Execute the 66 designed test cases. Log defects, fix them and retest affected cases, including boundary, concurrency and security checks. | [Artifact 6](06-test-cases.md), Artifact 5 requirements and Artifact 8 coverage. | System testing complete, results and retests recorded, no critical defects open and other UAT entry conditions met before UAT starts. |
| 5. UAT | Execute the nine business scenarios, starting with the role access check. Restore fictional sample data in the UAT environment and reconcile it to the source backup. Record outcomes, defect dispositions and retests. | [Artifact 7](07-uat-plan.md), the permissions matrix, Artifact 6 fictional sample data and the open items register. | Both operational checks succeed, the UAT exit criteria are met, and the Owner and Manager signs off. |
| 6. Delivery and Go Live | Prepare opening stock and customer balances from the reconciled source records, confirm catalogue prices and reorder levels, set up individual user accounts, and train staff briefly by role. Rehearse and run the old and new invoice processes in parallel for a short period, reconcile them, go live and provide short post go live support. | Artifacts 1 and 2 reconciliation dependencies and baselines, Artifact 5 roles and workflows, and Artifact 7 sign off. | Opening records reconcile, staff can carry out their assigned workflows, the new process is in use and the short support period has concluded. |

## Release 1 MVP and build order

The MVP serves internal staff at one electronics and accessories store with retail and wholesale customers. It covers the catalogue, stock receiving and adjustments, customer records, sales, invoice generation, payments, receivables, low stock, basic reporting, roles and permissions, and search and filtering. All 12 features are in the Release 1 baseline. Customers receive documents through staff.

| Build sequence | Stories and dependency |
| --- | --- |
| Accounts, role enforcement and audit foundations | US-21, US-20, US-22. Establish individual attribution and authorization before enabling any money or stock movement. |
| Catalogue and approved prices | US-01, US-02. Establish SKUs, reorder levels and approved retail and wholesale prices before the stock and sales workflows use them. |
| Stock receiving and adjustments | US-03, US-04, US-05. Post verified receipts and controlled adjustments to establish current stock for availability checks. |
| Customers and sales | US-06, US-07, US-08, US-09, US-10. Use approved customer-type prices and recheck current stock before completion. |
| Invoicing | US-11, US-12. Deliver one numbered, printable invoice per completed sale. Build and integrate issuance with completion and stock deduction as one commit under Artifact 5 rule 3. |
| Payments and settlement | US-13, US-14, US-15. Apply full or partial payments to issued invoices and maintain current balances and customer payment history. |
| Operational lists, reporting and search | US-16, US-17, US-18, US-19, US-23. Use posted records for receivables, low stock and reports. Add record searches and filters as their lists become available and enforce role access in the results. |
| Backup and recovery | US-24. Include daily backups in the build and have sample restore capability ready for system testing and UAT. |

## Later releases

**Release 2.** Supplier purchase orders, barcode support and customer statements. Scanner and label printer hardware integration is proposed for this release, subject to confirmation of the covered devices.

**Release 3.** Second location support, a forecasting report and customer portal evaluation. Stock transfers between locations and demand forecasting within the report are proposed.

**Release 4.** Proposed planning placeholder for portal implementation and an online store subject to evaluation and approval, card payment processing and payment gateways, accounting, payroll and tax filing, delivery scheduling and routing, and automatic reordering.

## Risks and dependencies

| Existing issue or dependency | Planned response |
| --- | --- |
| Open policies and workflow decisions can affect design and verification. | Use the 27-item register in Artifact 8 to confirm affected scope, required inputs and expected outcomes. In particular, reconcile the counter flow's stock deduction timing with Artifact 5 rule 3 when finalizing the diagram. |
| The current workflow baseline has yet to be measured. | Measure median invoice preparation time, completed sale lines against unavailable stock, and same-day visibility and reconciliation of unpaid balances. Agree the observation periods and invoice time improvement target with the owner, then measure the same outcomes after go live. |
| Opening quantities and balances can differ from physical stock and unpaid records. | Reconcile spreadsheets and paper records to physical counts, unpaid invoices and recorded payments before loading opening data. Confirm prices and reorder levels with the owner. |
| Staff may revert to paper under counter pressure. | Rehearse counter sales during acceptance, train by role and reconcile invoices, stock effects and balances during the parallel run. |
| Incorrect prices, unexplained stock movements or lost records can disrupt the store. | Review approved prices and approval evidence, inspect the audit trail for discrepancies, back up records daily and verify a sample restore before sign off. |
| Performance assumptions cover one store's catalogue and transaction history under normal concurrent staff use. | Verify routine screens and searches at that scope, including counter, stock and management use. Multi-location operation is future scope. |

## Training and adoption

The Sales Clerk rehearses product lookup, retail and wholesale sales, printing, and full and partial payments. The Inventory Clerk rehearses supplier receipts, mismatches, reason-coded adjustments and held approvals. The Owner and Manager rehearses price and adjustment approval, account administration, receivables, low stock, reports and audit review. Use the Role access check and the training rehearsal to confirm that each person can do their assigned work. In the parallel run, compare old and new invoice records, stock effects and customer balances and resolve differences before switching over.

## Release 1 Definition of Done

- All 12 baseline features and their functional and security requirements meet the agreed acceptance criteria. System tests, fixes and retests are recorded against the traceability matrix.
- All nine UAT scenarios have recorded outcomes. Role access and backup restore checks succeed, no critical or major defects remain open, and the owner accepts any remaining minor defects in writing. Open business decisions are settled or formally deferred under the UAT plan and the owner signs off.
- Opening stock and customer balances reconcile, catalogue data is confirmed, and individual staff accounts and training are in place. The parallel run is reconciled, the system goes live and the short support period concludes.

## Change log

| Version | Date | Change |
| --- | --- | --- |
| 1 | November 24, 2026 | Documented the Release 1 delivery phases, build order, later releases and delivery criteria. |
