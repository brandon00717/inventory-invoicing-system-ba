# User Acceptance Testing (UAT) Plan

**Project** Inventory and Invoicing System  
**Author** Brandon Narine  
**Date** November 24, 2026  
**Status** Phase 1 business analysis. The system has yet to be built. Business users will execute this plan against the built system in a later phase.

## Purpose and objectives

UAT will establish whether staff can run a trading day on Release 1, from receiving and managing stock through selling, issuing Invoices, taking payments and checking what Customers owe at close. The Owner and Manager will use the scenario outcomes and outstanding decisions to decide whether to accept Release 1.

| Business objective | What business users will verify |
| --- | --- |
| Reduce time to produce an invoice. | The Sales Clerk can take an ordinary counter sale from stock lookup to a ready-to-issue, itemized Invoice without preparing a separate document or recalculating the charges. Record workflow delays for review. The BRD's baseline and post-rollout comparison will establish the actual improvement against the target agreed by the Owner and Manager. |
| Eliminate sales recorded against stock that is unavailable. | The Sales Clerk can explain an availability limit to a Customer, correct the sale to the available quantity and complete it. Recorded stock reflects what was sold. |
| Give the owner a same-day view of unpaid balances. | By close, the Owner and Manager can use Receivables List to see current amounts owed, in collection order, and reconcile them with the Sales Clerk's Invoice and Payment totals. |

## Scope

Business users execute the scenarios in their own roles. Scope covers the Release 1 Product catalogue, stock receiving and adjustments, Customer records, sales, Invoices, Payments, Receivables List, Low Stock List, Basic reporting, roles and permissions, and authorized search and filtering. It includes the operational role access and backup restore checks required by the project spec. [BRD business objectives](01-business-requirements-document.md#business-objectives) · [Project spec](PROJECT_SPEC_v5.md)

Technical and system testing are covered by Artifact 6. Performance testing and the future-release features listed in Artifact 2 are outside UAT scope. [Test Cases](06-test-cases.md) · [Release 1 exclusions](02-stakeholder-and-scope-analysis.md#out-of-scope-for-release-1)

## Participants and responsibilities

| Participant | Responsibility |
| --- | --- |
| Owner and Manager | Validates catalogue maintenance, price and stock adjustment approvals, reports, receivables, low stock, User Accounts and audit evidence. Reviews restore evidence, decides defect disposition and open business decisions, and is the accepting authority. |
| Sales Clerk | Validates Customer records, counter sales, pricing and stock checks, Invoice printing, Payment recording and payment history. Supplies Invoice and Payment totals for reconciliation. |
| Inventory Clerk | Validates stock receipts, discrepancy handling, Stock Adjustments and reconciliation to counted stock. |
| UAT facilitator | Prepares sample records and sessions, briefs participants, records scenario outcomes and evidence, maintains the defect log, coordinates retests and prepares the sign off record. |
| Defect fixer | Investigates and fixes assigned defects, supplies a corrected build and identifies affected scenarios for business retest. |
| Authorized restore operator | Restores the sample backup in the UAT environment and makes recovery evidence available to the facilitator and business users. |

## Entry criteria

Before UAT starts:

- System testing is complete and no critical defects remain open.
- The UAT environment is ready, with the fictional Artifact 6 sample data loaded and an isolated restore destination available within UAT. Use its baseline for each scenario unless otherwise stated. Use October 4, 2026 for new fictional transactions.
- Each participant has an individual account in their real business role. A second authorized Owner and Manager is available for UAT-05. The authorized restore operator is available for UAT-09.
- Participants have been briefed on their roles, scenarios and how to raise issues. The participants agree to this plan.

The facilitator schedules both operational checks at entry. The checks are part of UAT and must be completed successfully before sign off.

- **Role access check (UAT-08).** Each participant signs in, confirms they can do their own work, and confirms that work outside their role is unavailable, using the approved permissions matrix. Start with this check before the main trading sessions.
- **Backup restore check (UAT-09).** Restore a backup in the UAT environment and verify restored business records against the source at backup time. Record the result and keep the check open until the restore and reconciliation succeed.

## UAT scenarios

Use the approved business rules and permissions matrix with the fictional [Artifact 6 sample data](06-test-cases.md#test-data). Record the observed business outcome, participant role, evidence and any defect references for each scenario. [Permissions matrix](05-functional-specification.md#roles-and-permissions-matrix)

### UAT-01 Run a normal trading day

**Validates** US-03, US-07, US-08, US-09, US-10, US-11, US-12, US-13, US-14, US-15, US-16, US-17, US-23 · BR-02, BR-04, BR-05, BR-06, BR-07, BR-08, BR-09, BR-10, BR-11, BR-12, BR-17, BR-20

1. At opening, the Owner and Manager checks Low Stock List and identifies Sample Adapter (ADP-01) at its reorder level of 1. The Inventory Clerk counts and receives all 3 Sample Cable units (CBL-01) from the delivery note, checks the posted receipt and verifies that stock rises from 5 to 8.
2. Sample Finch asks for 2 ADP-01. The Sales Clerk finds the Product and Customer using search and filters, sees the existing balance, and attempts the requested sale. With only 1 available, the clerk explains the shortage, agrees to sell the last unit and completes that sale at the retail price of G$3,000. The clerk records full payment and prints Invoice 105 showing paid and G$0 owed.
3. Sample Kestrel buys 2 CBL-01. The Sales Clerk completes the sale at the wholesale price of G$1,000 each. The clerk records G$1,500 against Invoice 106 and prints it showing partly paid and G$500 owed. The clerk finds the day's Sales and Invoices using search and filters, and checks the payments in each Customer's history. For both Customers, the clerk records only the amount against the Invoice and keeps card data out of all fields, including free text.
4. At close the Owner and Manager compares Receivables List with the Sales Clerk's settlement summary. The Owner and Manager also checks that ADP-01 remains on Low Stock List at zero.

**Success** The original request for 2 ADP-01 produces no Invoice or stock movement. Invoices 105 and 106 are itemized, printable and numbered in sequence. Stock falls once on completion, to 0 for ADP-01 and 6 for CBL-01, and stays there when Payments are recorded. Invoice 105 is absent from Receivables List. Invoice 106 is last, with G$500 owed. The existing unpaid and partly paid balances remain, and total receivables at close are G$11,000. Sample Finch owes G$8,500 and Sample Kestrel owes G$2,500. Payment and Customer handling satisfy the business practice in SEC-17.

### UAT-02 Return an item to stock

**Validates** US-04, US-22 · BR-03, BR-16, BR-19

1. Sample Finch returns one saleable CBL-01 from Invoice 101. The Sales Clerk locates the issued Invoice and confirms the item for the Inventory Clerk.
2. The Inventory Clerk counts the returned unit and records a +1 Stock Adjustment with the reason code **customer return**. When the clerk first leaves the reason blank, the adjustment is held from posting. The clerk selects the correct reason and submits it. The Owner and Manager checks the Stock Adjustment and Audit Trail View for the recorded reason, staff account, date and time.

**Success** The adjustment requires a reason, posts once and raises CBL-01 stock from 5 to 6. The Stock Adjustment and audit evidence explain the increase.

### UAT-03 Approve a higher value stock adjustment

**Validates** US-04, US-05, US-22 · BR-03, BR-16, BR-18, BR-19

1. The Inventory Clerk finds 3 damaged Sample Dock units (DCK-01) and submits a −3 Stock Adjustment with the reason code **damage**. The approval value is G$15,000 under rule 8, above the G$10,000 sample threshold. The clerk checks that the request is pending and stock still shows 12.
2. The Owner and Manager reviews the count, reason and approval value and approves the adjustment submitted by the Inventory Clerk. The clerk checks the updated stock. The Owner and Manager checks who submitted and approved it in Audit Trail View.
3. In a separate count review from baseline, the Inventory Clerk enters a −6 count correction for CBL-01 when the system shows 5 units. When the correction is blocked, the clerk returns to the physical count.

**Success** The damage adjustment changes stock from 12 to 9 once, after owner approval, with both staff accounts identified. A count correction that would take stock below zero cannot post.

### UAT-04 Confirm a delivery that comes up short

**Validates** US-03, US-23 · BR-02, BR-20

1. The Inventory Clerk expects 3 CBL-01 but counts 2. The clerk records both quantities, the receipt date and the note “One unit short on count.” The clerk checks that the receipt is held and Product List still shows 5.
2. After a recount confirms 2, the clerk reopens the held receipt, confirms the counted quantity and reviews the posted receipt and stock.
3. Separately, the clerk checks a delivery line for 3 Sample Splitter units (SPL-01) against the catalogue and refers it to the Owner and Manager when the receipt cannot post for the inactive Product.

**Success** The held receipt preserves the discrepancy and leaves stock at 5. Confirmation posts 2 units once, bringing stock to 7. The inactive Product remains at zero.

### UAT-05 Prepare the catalogue and approve a price change

**Validates** US-01, US-02, US-08, US-11, US-12, US-22 · BR-01, BR-05, BR-08, BR-16, BR-17, BR-19

1. The Owner and Manager prepares Sample Hub (HUB-01) for sale. The Owner and Manager starts with a copied SKU, CBL-01, and corrects it when the catalogue flags the duplicate. Save and review HUB-01 with the sample catalogue details. During the same catalogue review, update the SPL-01 reorder level to 0 and change its active status to Active, then restore its Inactive status. Propose G$3,000 retail and G$2,000 wholesale for HUB-01 and a new CBL-01 retail price of G$2,500.
2. Before approval, the Sales Clerk checks that new CBL-01 retail sale lines still use G$2,000 and that HUB-01 has no price available for sale. The second authorized Owner and Manager approves the proposals. The proposer reviews the price changes and the two accounts in Audit Trail View.
3. The Sales Clerk takes a new retail order for 1 CBL-01 from Sample Finch. If a line prepared before approval still shows the old price, the clerk refreshes it before completing the sale at G$2,500 and printing its Invoice. The clerk then finds and reprints Invoice 101 to answer a question about its original charge.

**Success** The existing CBL-01 record survives the duplicate attempt, and the catalogue changes are saved on the intended records. Prices become usable when approved by the separate authorized account. The next CBL-01 retail line uses G$2,500, the wholesale price stays G$1,000, and Invoice 101 keeps its number, G$2,000 unit price and G$4,000 total.

### UAT-06 Reconcile receivables, stock and sales

**Validates** US-13, US-14, US-15, US-16, US-17, US-18, US-19, US-22 · BR-09, BR-10, BR-11, BR-12, BR-13, BR-14, BR-16, BR-19

1. The Owner and Manager reviews Receivables List, oldest first with Invoice number breaking same-date ties. The Sales Clerk records the G$1,500 partial payment against Invoice 102. For Invoice 101 the clerk corrects a tendered G$2,500.01 to the exact G$2,500 owed after the excess is blocked. The clerk checks payment history and prints the updated Invoice if requested. The Owner and Manager refreshes Receivables List that day and checks Payment entries in Audit Trail View.
2. The Owner and Manager uses Low Stock List to identify ADP-01 at its reorder level. The Inventory Clerk receives the matching 3 CBL-01 delivery and confirms the quantity now on hand for the stock review.
3. The Sales Clerk totals the issued Invoices for October 1–2, 2026 and tallies item quantities and amounts across the four baseline Invoices. Use the incomplete October 2 draft from Artifact 6 to confirm that open counter work does not enter either report. The Owner and Manager compares the clerk's period total with Sales by period, compares the item tally with Sales by product, and uses Audit Trail View for any follow-up. The Sales Clerk handles individual Invoice lookups.
4. The Owner and Manager runs Stock on hand value. If the valuation basis and required inputs have been confirmed, compare the stated basis and total with an independent calculation from the current quantities. If the basis or inputs are outstanding, verify that the value is withheld and record the unverified valuation outcome for the owner's sign off decision.

**Success** The list shows Invoice 102 at G$4,500 and Invoice 104 at G$2,000, in that order. Invoice 101 has dropped off, and paid Invoice 103 stays off. The period sales total is G$11,000 for Invoices 101–103. Sales by product agrees with the issued Invoice tally: CBL-01 shows 3 units and G$5,000, DCK-01 shows 1 unit and G$6,000, and ADP-01 shows 1 unit and G$2,000. The incomplete draft contributes no sales. The stock value reconciles when the basis and inputs are confirmed. Any outstanding valuation decision and verification are documented for sign off.

### UAT-07 Onboard a new retail Customer

**Validates** US-06, US-07, US-11, US-23 · BR-04, BR-05, BR-08, BR-20

1. Sample Tern asks for a quote at the counter and provides contact details. The Sales Clerk creates the retail Customer using the C3 sample details, then finds the Customer by name and retail type. The clerk checks the saved details, using only the information needed for invoicing and contact.
2. The clerk selects Sample Tern and starts preparing a sale. The Customer leaves before agreeing what to buy. The clerk checks that the sale cannot complete without an item and abandons it without issuing an Invoice.
3. Sample Tern returns for 1 CBL-01. The clerk finds the existing Customer, completes the sale at G$2,000 and issues the Invoice. In a separate follow-up, the clerk updates Sample Finch using the Sample Finch Annex details and checks that the existing balance is preserved.

**Success** The saved Customer can be found and reused with the correct type and contact details. The abandoned sale leaves the Customer balance, stock and Invoice sequence unchanged. The completed sale issues Invoice 105 and records G$2,000 owed. The Sample Finch Annex edit saves the updated details and leaves its balance at G$8,500. The workflow uses only the Customer details permitted by the business rules and collects no card data.

### UAT-08 Role access check

**Validates** US-20, US-21, US-22, US-23 · BR-15, BR-16, BR-19, BR-20

1. Each participant signs in on their own account, opens Dashboard and uses the permissions matrix to confirm which work is available. The Owner and Manager checks distinct staff accounts and assignments in User Accounts, confirms access to approvals, summary views and Audit Trail View, and verifies that individual Customer and Invoice views, sales, Payments and stock receipts are unavailable. Use the matrix's single-authorized-user condition when checking whether the Owner and Manager can record a Stock Adjustment.
2. The Sales Clerk finds a Product with approved prices, opens a Customer and an Invoice and checks access to normal sale and payment work. Confirm that catalogue changes, price and Stock Adjustment approvals, stock entry and owner-only lists or reports are unavailable. The Inventory Clerk checks catalogue details and stock, receives stock and records an adjustment, and confirms prices are unavailable on Product List and sales, Customer, Invoice and owner-only functions are unavailable.
3. Each role signs out. Record the result against each matrix row. The Owner and Manager checks that completed sample actions are attributable to the individual accounts. Confirm the Owner and Manager can view audit entries and that all roles are barred from changing or deleting them.

**Success** Each participant can perform their assigned work and reach only the functions, records and fields permitted by the approved matrix. Individual staff actions can be attributed and audit entries remain protected.

### UAT-09 Backup restore check

**Validates** US-24 · BR-21

1. The Owner and Manager reviews dated daily backup outcomes for two consecutive UAT operational days. The facilitator keeps the source snapshot for the selected daily backup of Artifact 6 baseline data. The authorized restore operator restores that backup to the isolated destination in the UAT environment.
2. The Inventory Clerk checks restored stock against the source. The Sales Clerk checks Customer details and balances, finds the restored Sales, and uses their Invoices to compare items, amounts and Payments against the source snapshot. The Owner and Manager reviews the recovery evidence and compares the restored Receivables List with the source totals and the clerks' reconciliation. The facilitator records variances and references to the evidence.
3. If a restore fails or figures differ, record the issue and repeat the check after correction, using the same backup source for comparison.

**Success** The restored sample matches the source. Quantities for CBL-01, DCK-01, ADP-01 and SPL-01 are 5, 12, 1 and 0. Sales S1–S4 and Invoices 101–104 retain their items and totals. Payments of G$1,500 on Invoice 101 and G$1,000 on Invoice 103 retain their links. Sample Finch and Sample Kestrel balances reconcile to G$8,500 and G$2,000. The check stays open until restore and reconciliation both succeed.

## Defect handling

The facilitator records each issue in a defect log with an ID, description, scenario ID, person who raised it, expected and observed outcomes, evidence, severity, assignee and status.

| Severity | Definition |
| --- | --- |
| Critical | Stops the business process with no workable way to complete it. |
| Major | Disrupts the business process and has a workaround. |
| Minor | Cosmetic or wording issue that does not change the business outcome. |

The Owner and Manager agrees severity and decides which fixes are required with input from the facilitator, participants and defect fixer. After a fix, the business participant reruns the affected scenario from its agreed starting state. The facilitator records the result and marks a defect-affected scenario accepted only after a clean retest. Any outstanding minor issue requires written owner acceptance at sign off.

## Schedule outline

| Phase | Working days | Planned work |
| --- | --- | --- |
| Preparation | Day 1 | Confirm entry criteria, sample records, accounts, restore destination and participant briefing. |
| Scenario execution | Days 2–3 | Open with the role access check. Run the trading, Customer and Invoice sessions with the Sales Clerk, stock sessions with the Inventory Clerk, and approval, reporting and restore review with the Owner and Manager. Capture daily backup outcomes during UAT. |
| Defect fixes and retest | Days 4–5 | Implement agreed fixes, rerun affected scenarios with the responsible roles and reconcile any repeated restore. Extend the window if findings warrant it. |
| Sign off | Day 6 | Review evidence, defect dispositions and business decisions. Sign once the exit criteria are met. |

## Exit criteria and sign off

- All scenarios have been executed and their outcomes recorded. Both operational checks have succeeded, and every scenario affected by a fixed defect has a clean retest.
- No critical or major defect remains open. Any remaining minor defects are listed and accepted in writing by the Owner and Manager.
- The Owner and Manager has settled or formally deferred the open business decisions carried forward from the earlier artifacts, including BR-14 stock on hand valuation and US-20 permissions confirmation. Record each decision or deferral in the sign off record. Review the Artifact 2 scope questions alongside the open decisions in the BRD, User Stories and Functional Specification. For a deferral, record the affected scope, any verification still due and when the decision will be revisited.

**Statement of acceptance**  
I accept Release 1 based on the recorded UAT outcomes, the minor defects I have accepted in writing and the documented disposition of open business decisions.

**Owner and Manager name** ________________________________________  
**Owner and Manager signature** ___________________________________  
**Date** __________________________________________________________  
**UAT facilitator** _______________________________________________

## Change log

| Version | Date | Change |
| --- | --- | --- |
| 1 | November 24, 2026 | Initial Release 1 UAT plan. |
