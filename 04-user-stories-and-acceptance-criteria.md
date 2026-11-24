# User Stories and Acceptance Criteria

**Project** Inventory and Invoicing System  
**Author** Brandon Narine  
**Date** November 24, 2026  
**Status** Draft for Phase 1 business analysis. The system has yet to be built.

## Definition of Ready

- The story names an approved internal persona, links to one or more business requirements, and has a Release 1 priority and a Fibonacci estimate.
- Given, When, Then criteria specify observable results, including rejection or boundary behavior where applicable.
- Dependencies and open business questions are identified. Any proposed default comes from the approved baseline. Decisions needed to test the story are confirmed before development.

## Definition of Done

- The implementation passes the story's acceptance criteria using fictional sample records, including applicable stock, invoice, payment, and role checks.
- Required audit trail entries identify the responsible account or sign-in identity, the event, and when it occurred. Approval checks pass where the story requires them.
- Affected documentation and traceability are updated, and the business reviewer accepts the result.

## Release 1 user stories

### US-01 Maintain the Product catalogue

As an Owner and Manager, I want to maintain product records, so that staff have a single reference for products and stock.

**Linked requirements** BR-01 · **MoSCoW** Must have · **Points** 3

- **Given** valid product details and an unused SKU, **When** the Owner and Manager adds the product, **Then** the record shows its SKU, name, category, unit, approved retail price, approved wholesale price, quantity on hand, reorder level, and active status.
- **Given** an existing product, **When** the Owner and Manager attempts to add another product with the same SKU, **Then** the system rejects the duplicate and preserves the existing record.
- **Given** an active product, **When** the Owner and Manager changes its reorder level and active status, **Then** the product record shows both saved changes.

**Dependency.** US-02 governs approval of retail price and wholesale price. Opening quantity on hand depends on reconciliation to physical counts.

### US-02 Approve product prices and price changes

As an Owner and Manager, I want to approve product prices and price changes, so that staff charge the prices the business has authorized.

**Linked requirements** BR-01, BR-17, BR-19 · **MoSCoW** Must have · **Points** 5

- **Given** a proposed initial retail price or wholesale price, **When** it is presented for use before approval, **Then** it is unavailable for sale pricing.
- **Given** a proposed price change, **When** the required approval is recorded under the Owner and Manager's oversight, **Then** the approved price becomes available for new sales and the audit trail identifies the change and approval.
- **Given** a Sales Clerk without price-change permission, **When** the Sales Clerk attempts to change a product price, **Then** access is denied and the approved price remains in effect.
- **Given** a second authorized user is available, **When** the user who created the price proposal tries to approve it, **Then** approval is blocked and the proposal remains unapproved.

**Dependency.** The Owner and Manager confirms price-change permissions and the approval workflow, including how to handle cases with one authorized user.

### US-03 Record stock received against a supplier delivery

As an Inventory Clerk, I want to record stock received against a supplier delivery, so that increases in stock can be reconciled to deliveries.

**Linked requirements** BR-02 · **MoSCoW** Must have · **Points** 3

- **Given** an active product and a quantity received that matches the delivery note, **When** the Inventory Clerk records the receipt, **Then** the supplier delivery record shows the product SKU, quantity received, and receipt date, and quantity on hand increases by the quantity received.
- **Given** the counted quantity and the delivery note disagree, **When** the Inventory Clerk compares them during receiving, **Then** the clerk identifies the mismatch and holds the receipt pending resolution, with no change to quantity on hand.
- **Given** an inactive product, **When** the Inventory Clerk attempts to record a receipt for it, **Then** the system rejects the receipt and leaves quantity on hand unchanged.

**Open item.** Confirm how mismatches are resolved and whether multiple products must be grouped under one supplier delivery.

### US-04 Record a stock adjustment with a reason code

As an Inventory Clerk, I want to record a stock adjustment with a reason code, so that the business can explain a difference in quantity on hand.

**Linked requirements** BR-03, BR-18, BR-19 · **MoSCoW** Must have · **Points** 3

- **Given** a valid quantity change that does not require approval, **When** the Inventory Clerk records the adjustment with an approved reason code, **Then** quantity on hand reflects the adjustment and the entry records the reason code, responsible account, date, and time.
- **Given** an adjustment with a missing or unrecognized reason code, **When** the Inventory Clerk submits it, **Then** the system rejects the adjustment and quantity on hand stays unchanged.

The approved reason codes are damage, loss, count correction, return to supplier, and customer return. US-05 governs adjustments above the approval threshold.

### US-05 Approve higher value stock adjustments

As an Owner and Manager, I want to review and approve stock adjustments above the threshold, so that higher value changes are checked before they affect recorded stock.

**Linked requirements** BR-18, BR-19 · **MoSCoW** Must have · **Points** 5

- **Given** an otherwise valid adjustment whose value exceeds the threshold, **When** the Inventory Clerk submits it, **Then** it awaits owner approval and quantity on hand stays unchanged.
- **Given** a pending adjustment above the threshold, **When** the required owner approval is recorded, **Then** quantity on hand reflects the approved quantity change and the audit trail identifies who recorded and who approved it and when.
- **Given** a second authorized user is available, **When** the user who recorded the adjustment tries to approve it, **Then** the system blocks approval and quantity on hand stays unchanged.
- **Given** an otherwise valid adjustment valued exactly at the threshold, **When** the Inventory Clerk submits it, **Then** the above-threshold approval rule does not apply.

**Open question.** Who can approve a stock adjustment above a set value, and what is that value? Use the Artifact 2 default of owner approval above G$10,000, an invented threshold for analysis, and a separate authorized approver whenever a second user is available. The basis for determining adjustment value and the workflow with only one authorized user need confirmation.

### US-06 Maintain Customer records

As a Sales Clerk, I want to maintain Customer records, so that I can select the correct Customer for invoicing and follow-up.

**Linked requirements** BR-04 · **MoSCoW** Must have · **Points** 2

- **Given** a Customer's name, type of retail or wholesale, and contact details needed for invoicing and contact, **When** the Sales Clerk saves the record, **Then** it is available for sale selection with those details and shows the balance owed.
- **Given** an existing Customer record, **When** the Sales Clerk updates its contact details, **Then** the updated details are available and the balance owed still reflects invoices and payments.

**Dependency.** Opening balances depend on reconciliation to unpaid invoices and payment records. US-14 covers ongoing balance updates.

### US-07 Create a Sale

As a Sales Clerk, I want to select a Customer and enter product lines, so that each sale records what the Customer is buying and at what price.

**Linked requirements** BR-04, BR-05 · **MoSCoW** Must have · **Points** 5

- **Given** a selected Customer and valid quantities and approved prices, **When** the Sales Clerk completes a sale that passes stock validation, **Then** the sale is linked to that Customer and records each product, quantity, and unit price.
- **Given** a sale with no selected Customer or no product lines, **When** the Sales Clerk attempts to complete it, **Then** the system blocks completion and produces no invoice.
- **Given** a product line with zero or negative quantity, **When** the Sales Clerk attempts to complete the sale, **Then** the system rejects the affected line and blocks completion until corrected.
- **Given** a Customer with an unpaid balance, **When** the Sales Clerk selects that Customer and enters a sale that meets the other completion criteria, **Then** the system shows the existing balance at selection and allows the sale to complete.

**Open question.** Can staff complete a sale for a customer with an unpaid balance? Apply the Artifact 2 default of allowing the sale and showing the existing balance during selection. Completion depends on US-08 through US-11.

### US-08 Apply the approved price for the Customer type

As a Sales Clerk, I want the sale to use the approved price for the Customer type, so that the Customer is charged the applicable price.

**Linked requirements** BR-05, BR-17 · **MoSCoW** Must have · **Points** 3

- **Given** a retail Customer and a product with an approved retail price, **When** the Sales Clerk adds the product to the sale, **Then** the unit price uses the retail price.
- **Given** a wholesale Customer and a product with an approved wholesale price, **When** the Sales Clerk adds the product to the sale, **Then** the unit price uses the wholesale price on the product record.
- **Given** a sale line with a unit price that differs from the approved price for the selected Customer type, **When** the Sales Clerk attempts to complete the sale, **Then** completion is blocked until the line uses the applicable approved price.

**Open question.** Is the wholesale price fixed per product or negotiated for each customer? Apply the Artifact 2 default of one approved wholesale price per product for every wholesale Customer.

### US-09 Validate stock availability

As a Sales Clerk, I want the system to check stock before completing a sale, so that I can sell only what the store has on hand.

**Linked requirements** BR-06 · **MoSCoW** Must have · **Points** 5

- **Given** a valid sale requesting the last available unit of a product, **When** the Sales Clerk submits it for completion, **Then** the stock check passes.
- **Given** a requested quantity greater than stock on hand at completion, **When** the system validates the sale, **Then** it blocks the affected line, shows that stock is insufficient, and prevents completion.
- **Given** that more than one line requests the same product and the combined quantity exceeds stock on hand, **When** the system validates the sale, **Then** it blocks completion until the total requested quantity fits the available stock.
- **Given** a blocked line, **When** the Sales Clerk adjusts or removes it, **Then** the system validates the remaining lines using stock available at completion. If no lines remain, the sale ends without an invoice.

### US-10 Deduct stock at sale completion

As an Inventory Clerk, I want completed sales to deduct sold quantities automatically, so that quantity on hand reflects the units still in the store.

**Linked requirements** BR-07 · **MoSCoW** Must have · **Points** 5

- **Given** a sale that passes validation, **When** it is completed, **Then** quantity on hand decreases by the total quantity sold for each product.
- **Given** a validated sale for which no payment will be taken at the counter, **When** the sale completes, **Then** quantity on hand decreases by the sold quantities at completion.
- **Given** a completed sale whose stock deduction has already been applied, **When** a payment is recorded against its invoice, **Then** no additional units are deducted for that sale.

**Dependency.** Sale completion follows US-07 through US-09. The timing assumption from the Task 3 audit is recorded in UC-01.

### US-11 Generate the numbered invoice

As a Sales Clerk, I want each completed sale to produce its own numbered invoice, so that I can give the Customer a consistent record of charges.

**Linked requirements** BR-08 · **MoSCoW** Must have · **Points** 5

- **Given** a completed sale with valid sale lines, **When** its invoice is generated, **Then** the system assigns the next unique sequential invoice number and presents the sale lines, system-calculated totals based on the recorded quantities and unit prices, and the payment status. With no payment recorded, the status is unpaid.
- **Given** a sale with no completed sale lines, **When** invoice generation is attempted, **Then** the system rejects the request and issues no invoice.
- **Given** that a completed sale already has an invoice, **When** the Sales Clerk requests its invoice again, **Then** the existing invoice and invoice number are returned and no second invoice is issued for that sale.
- **Given** the proposed Release 1 tax display default, **When** the system generates an invoice, **Then** the invoice omits a separate tax line.

**Open question.** Must tax appear on the invoice, and what description should appear if it does? Apply the Artifact 2 default of omitting a separate tax line from the fictional draft invoice pending confirmation.

### US-12 Print the invoice

As a Sales Clerk, I want a printable invoice view, so that I can give the Customer a paper copy of the invoice.

**Linked requirements** BR-08, BR-10 · **MoSCoW** Must have · **Points** 2

- **Given** an issued invoice, **When** the Sales Clerk opens its printable view, **Then** the view shows the invoice number, itemized sale lines, calculated totals, and current payment status.
- **Given** that a partial or full payment was recorded after the invoice was issued, **When** the Sales Clerk prints the invoice again, **Then** the printout shows the updated payment status and the original invoice number.
- **Given** a sale with no issued invoice, **When** the Sales Clerk requests its printable invoice, **Then** no invoice is available to print.

**Open question.** Do customers need printed invoices, emailed invoices, or both? Apply the Artifact 2 default of printed invoices using the Release 1 printable view.

### US-13 Record full and partial payments against an invoice

As a Sales Clerk, I want to record a full or partial payment against an invoice, so that each payment is applied to the debt it settles.

**Linked requirements** BR-09, BR-19 · **MoSCoW** Must have · **Points** 5

- **Given** an invoice with an outstanding balance, **When** the Sales Clerk records a positive payment below that balance, **Then** the payment is recorded against that invoice and only that amount is applied.
- **Given** an unpaid or partly paid invoice, **When** the Sales Clerk records a payment equal to the remaining balance, **Then** the payment is recorded against that invoice and settles the amount due.
- **Given** a payment with a missing or invalid invoice number or a zero or negative amount, **When** the Sales Clerk submits it, **Then** the system rejects it and does not change any invoice balance.
- **Given** a valid payment, **When** the Sales Clerk records it, **Then** the payment record identifies the invoice number, amount, recorded by User ID, and payment date and time, and the audit trail records the event.

**Open item.** The business has yet to confirm how to handle an amount above the remaining invoice balance.

### US-14 Update invoice payment status and balance owed

As a Sales Clerk, I want payment status and balance owed to update when a payment is recorded, so that I can tell the Customer how much remains due.

**Linked requirements** BR-04, BR-10 · **MoSCoW** Must have · **Points** 3

- **Given** an unpaid invoice, **When** a partial payment is recorded, **Then** its payment status becomes partly paid, its amount owed equals the invoice total less the recorded payment, and the Customer's balance owed decreases by that payment amount.
- **Given** an unpaid or partly paid invoice, **When** a payment settles the remaining amount, **Then** its payment status becomes paid and its amount owed is zero. Any other outstanding invoices remain included in the Customer's balance owed.
- **Given** a completed sale for which no payment is recorded, **When** the invoice is available, **Then** its payment status is unpaid and its full amount due is included in the Customer's balance owed.

### US-15 Review payment history by Customer

As a Sales Clerk, I want to find payment history by Customer, so that I can confirm which payments were recorded against each invoice.

**Linked requirements** BR-09, BR-10 · **MoSCoW** Must have · **Points** 3

- **Given** payments against more than one invoice for a Customer, **When** the Sales Clerk opens that Customer's payment history, **Then** it shows the invoice number, amount, and payment date and time for each payment and excludes other Customers' payments.
- **Given** a recorded payment during Release 1, **When** the Sales Clerk revisits the payment history or its associated invoice, **Then** the payment and invoice remain available, including after full settlement.
- **Given** a Customer asks for confirmation of a recorded payment, **When** the Sales Clerk reviews the payment history and prints the updated invoice, **Then** the history identifies the payment and the printout shows the current payment status.

**Open questions.** How long must invoices and payment records be kept? Retain them throughout Release 1 and defer automatic deletion pending confirmation. Do customers require a separate payment receipt for each payment? Use the updated printed invoice and payment history for confirmation under the Artifact 2 default.

### US-16 View the Receivables list

As an Owner and Manager, I want to see unpaid and partly paid invoices by age, so that I can prioritize collection using current amounts owed.

**Linked requirements** BR-11 · **MoSCoW** Must have · **Points** 3

- **Given** unpaid, partly paid, and paid invoices with different issue dates, **When** the Owner and Manager views the Receivables list, **Then** it shows only unpaid and partly paid invoices, ordered by age since issue date, with their current amounts owed.
- **Given** an invoice on the list, **When** a partial payment is recorded and the list is viewed again on the same business day, **Then** the invoice is still listed with its reduced amount owed.
- **Given** an invoice on the list, **When** its remaining balance is paid and the list is viewed again on the same business day, **Then** the invoice is absent.

**Dependency.** The list uses the payment status and balances maintained in US-14. Sort direction and tie-breaking are open.

### US-17 View the Low stock list

As an Owner and Manager, I want to see products at or below their reorder level, so that I can identify potential shortages.

**Linked requirements** BR-12 · **MoSCoW** Must have · **Points** 2

- **Given** products with quantity on hand below, equal to, and above their respective reorder levels, **When** the Owner and Manager checks the Low stock list, **Then** the products below and equal to their reorder levels appear and the product above its reorder level does not.
- **Given** a sale deduction takes a product to its reorder level and a subsequent receipt takes it above that level, **When** the list is checked after each stock movement, **Then** the product appears after the sale and is absent after the receipt.

**Dependency.** The list uses confirmed reorder levels and the updated quantity on hand from stock movements.

### US-18 Review sales by period and by product

As an Owner and Manager, I want sales summaries by period and by product, so that I can see when sales occur and which products sell.

**Linked requirements** BR-13 · **MoSCoW** Could have · **Points** 3

- **Given** completed sales inside and outside a selected period, **When** the Owner and Manager runs the sales by period report, **Then** the reported totals reconcile to the completed sales in that period.
- **Given** completed sales containing different products, **When** the Owner and Manager runs the sales by product report, **Then** each product's reported quantities and sales amounts reconcile to its sale lines.

### US-19 Review stock on hand value

As an Owner and Manager, I want to see stock on hand value using the approved valuation basis, so that I can see how much value the store holds in inventory.

**Linked requirements** BR-14 · **MoSCoW** Could have · **Points** 3

- **Given** products with reconciled quantities and the inputs required by the approved valuation basis, **When** the Owner and Manager runs the stock on hand value report, **Then** the total reconciles to a calculation using that basis and the recorded quantities.
- **Given** stock has changed and the valuation inputs remain valid, **When** the Owner and Manager runs the report again, **Then** the total reflects the updated quantity on hand using the same approved valuation basis.

**Open question.** Which valuation basis should the owner use for the stock on hand value report? The BRD leaves it open. The owner must confirm it before this story is ready.

### US-20 Enforce User roles and permissions

As an Owner and Manager, I want the system to enforce approved staff permissions, so that only authorized staff can act on business records.

**Linked requirements** BR-15 · **MoSCoW** Must have · **Points** 5

- **Given** individual accounts assigned the Owner, Sales Clerk, and Inventory Clerk roles, **When** each user signs in and attempts approved functions for that role, **Then** the system allows the approved functions.
- **Given** a signed-in Sales Clerk without access to the owner-only price change function, **When** the Sales Clerk requests that function directly, **Then** the system denies the request without changing the product price.
- **Given** a user is signed out, **When** the user requests an invoice directly, **Then** the system redirects to sign-in and reveals no invoice details.

**Dependency.** The Owner and Manager confirms the roles and permissions. US-21 supplies the individual staff accounts.

### US-21 Manage individual staff accounts

As an Owner and Manager, I want each staff member to have an individual account, so that activity is attributable and staff access can be managed.

**Linked requirements** BR-15, BR-16 · **MoSCoW** Must have · **Points** 5

- **Given** two staff members with the same approved role, **When** the Owner and Manager creates their accounts, **Then** each has a distinct sign-in identifier and User ID.
- **Given** staff using different individual accounts, **When** each completes an authorized sale, stock adjustment, or payment, **Then** each record identifies the account responsible for that action.
- **Given** a staff account whose approved access changes, **When** the Owner and Manager updates that access, **Then** subsequent requests follow the updated permissions and previously recorded activity remains attributable to the original account.

### US-22 Keep a protected audit trail

As an Owner and Manager, I want to review a protected audit trail, so that I can investigate stock changes, disputed transactions, and failed sign-ins.

**Linked requirements** BR-16, BR-19 · **MoSCoW** Must have · **Points** 5

- **Given** an authorized staff account completes a sale, records a stock adjustment, changes a price with approval, or records a payment, **When** the Owner and Manager reviews the audit trail, **Then** each action has an entry identifying the event, the responsible account, the date, and the time.
- **Given** a failed sign-in attempt, **When** the Owner and Manager reviews the audit trail, **Then** the entry identifies the failure, the attempted sign-in identity, the date, and the time.
- **Given** existing audit entries, **When** a regular user attempts to edit or delete them, **Then** the system rejects the request and preserves the entries.

**Dependency.** Individual attribution uses US-21. Approval evidence uses US-02 and US-05.

### US-23 Search and filter records

As a Sales Clerk, I want to search and filter products, customers, sales, and invoices, so that I can find the records needed for counter service and follow-up.

**Linked requirements** BR-20 · **MoSCoW** Should have · **Points** 3

- **Given** matching and nonmatching authorized records of each of the four record types, **When** the Sales Clerk applies a search and filter to each type, **Then** the results contain the authorized records that meet both conditions.
- **Given** a search or filter matches a record outside the user's approved access, **When** the user runs it, **Then** that record is excluded from the results.

**Dependency.** Search and filtering use the access rules in US-20.

### US-24 Back up and restore business records

As an Owner and Manager, I want daily backups and proof that records can be restored, so that the business can recover its records after a loss or failure.

**Linked requirements** BR-21 · **MoSCoW** Must have · **Points** 5

- **Given** the system contains business records, **When** daily backup records are reviewed across consecutive days of operation, **Then** there is a completed backup of the business records for each day.
- **Given** a backup of fictional sample records, **When** the Owner and Manager verifies a restore before Release 1 sign off, **Then** the restored stock, Customer records, sales, invoices, payments, and balances reconcile to the backed-up sample records.
- **Given** a failed restore or a reconciliation discrepancy during the check, **When** the result is reviewed for sign off, **Then** the restore criterion stays open until a successful restore is demonstrated.

**Sizing note.** Estimates are relative. US-01 anchors 3 points for a bounded workflow with several validations. A 1 or 2 is smaller. A 5 or 8 reflects progressively more interactions, rules, or uncertainty.

**Coverage note.** The stories cover all 12 Release 1 features and all 21 business requirements. Future release assignments follow the approved [Artifact 2 scope statement](02-stakeholder-and-scope-analysis.md#out-of-scope-for-release-1).

| BR | Stories | BR | Stories | BR | Stories |
| --- | --- | --- | --- | --- | --- |
| BR-01 | US-01, US-02 | BR-08 | US-11, US-12 | BR-15 | US-20, US-21 |
| BR-02 | US-03 | BR-09 | US-13, US-15 | BR-16 | US-21, US-22 |
| BR-03 | US-04 | BR-10 | US-12, US-14, US-15 | BR-17 | US-02, US-08 |
| BR-04 | US-06, US-07, US-14 | BR-11 | US-16 | BR-18 | US-04, US-05 |
| BR-05 | US-07, US-08 | BR-12 | US-17 | BR-19 | US-02, US-04, US-05, US-13, US-22 |
| BR-06 | US-09 | BR-13 | US-18 | BR-20 | US-23 |
| BR-07 | US-10 | BR-14 | US-19 | BR-21 | US-24 |

## Structured use cases

### UC-01 Create a Sale

**Primary actor** Sales Clerk  
**Goal** Complete a validated sale, issue its invoice, and record any payment taken at the counter.  
**Preconditions** The Sales Clerk is signed in and authorized to create sales. The Customer record and the products to be sold are available, with approved prices and current quantity on hand.

**Main flow**

1. The Customer requests a product. The Sales Clerk looks up the product, creates the sale, selects the Customer, and enters product lines and requested quantities.
2. The system confirms that the sale contains at least one line and validates each line's quantity and unit price. For a retail Customer it uses the approved retail price.
3. The system validates the quantities against stock on hand before completion. All requested quantities are available.
4. The system completes the sale, invokes UC-02 to generate the invoice, and deducts the sold quantities once as part of sale completion.
5. The Customer pays the full invoice amount. The Sales Clerk records the payment against the invoice.
6. The system updates the invoice payment status and the Customer's balance owed. The Low stock list reflects products at or below their reorder level after the sale.

**Alternate flows**

- **Wholesale price, step 2.** For a wholesale Customer, the system uses the approved wholesale price on the product record. Continue at step 3.
- **Partial payment, step 5.** The Customer pays less than the amount due. The Sales Clerk records that payment. At step 6, the invoice becomes partly paid and its remaining amount is included in the Customer's balance owed.
- **No payment at the counter, step 5.** The invoice remains unpaid and the full amount due is included in the Customer's balance owed. The Low stock list reflects the stock deduction from step 4.

**Exception flows**

- **Out of stock, step 3.** The system blocks any line whose requested quantity exceeds available stock and shows a message. The Sales Clerk adjusts or removes the blocked line and returns to step 2. If no lines remain, the flow ends without a completed sale or invoice.
- **Invalid quantity or price, step 2.** The system blocks completion until affected lines are corrected or removed. Resume at step 2 after the Sales Clerk makes the changes.

**Postconditions** A completed sale has one invoice and a single deduction of its sold quantities. Any counter payment is linked to that invoice. Payment status, balance owed, and low stock flags reflect the recorded sale and any payment. If validation prevents completion, the system issues no invoice and deducts no stock.

**Timing assumption.** The Mermaid counter flow shows deduction at F13 after payment recording. Per BR-07, the deduction belongs to sale completion in every path, including when no payment is taken at the counter. A later payment never causes another deduction.

### UC-02 Generate an Invoice

**Primary actor** Sales Clerk  
**Goal** Obtain a printable, itemized invoice for a completed sale.  
**Preconditions** The Sales Clerk is signed in and authorized to invoice. The sale is completed and has at least one valid sale line for the main flow.

**Main flow**

1. As part of UC-01, the system receives the completed sale for invoicing and confirms it has completed sale lines.
2. The system calculates invoice totals from the sale quantities and unit prices and assigns the next unique sequential invoice number. Under the proposed default in US-11, it omits a separate tax line.
3. The system creates the invoice linked to the sale. It shows the invoice number, itemized sale lines, calculated totals, and payment status, which is unpaid before any payment is recorded.
4. The Sales Clerk opens the printable invoice view and prints the invoice for the Customer.

**Alternate flows**

- **Existing invoice, step 1.** If an invoice already exists for the sale, the system opens that invoice with the original invoice number and its current payment status. Continue at step 4.

**Exception flows**

- **Sale cannot be invoiced, step 1.** If the sale is incomplete or has no completed sale lines, the system rejects the request and issues no invoice.

**Postconditions** A successful request yields one printable invoice for the sale. Its invoice number identifies the record against which any payments are recorded. A rejected request creates no invoice.

### UC-03 Receive Stock

**Primary actor** Inventory Clerk  
**Goal** Record stock received against a supplier delivery and update quantity on hand.  
**Preconditions** The Inventory Clerk is signed in and authorized to receive stock. A delivery note and the physical delivery are available, and the product exists in the Product catalogue.

**Main flow**

1. The Inventory Clerk selects the product against the supplier delivery. The system checks that the product is active.
2. The Inventory Clerk counts the delivered product and compares the count to the delivery note. The quantities match.
3. The Inventory Clerk enters the quantity received and receipt date for the product and submits the receipt.
4. The system records the supplier delivery against the product SKU, increases quantity on hand by the quantity received, and reflects the new quantity in the Low stock list.

**Alternate flows**

- **Correct entry before posting, step 3.** If the entered quantity or date differs from the verified receipt details, the Inventory Clerk corrects it before submitting. Continue at step 4 after submission.

**Exception flows**

- **Quantity mismatch, step 2.** If the count differs from the delivery note, the Inventory Clerk identifies the discrepancy and holds the receipt for resolution. Quantity on hand stays unchanged for that receipt. Resume at step 2 when verified receipt details are available.
- **Inactive product, step 1.** If the product is inactive, the system blocks receipt for that product. No receipt is posted and quantity on hand stays unchanged.

**Postconditions** A posted receipt can be reconciled to the supplier delivery, product SKU, quantity received, and receipt date. Quantity on hand includes the posted receipt. A held or blocked receipt has no stock effect.

**Process reference.** The Mermaid future state flow covers the counter sale. This receiving flow follows BR-02 and the Receive Stock use case outline in Artifact 3.

## Change log

| Version | Date | Change |
| --- | --- | --- |
| 1 | November 24, 2026 | Initial draft of the Release 1 user stories, acceptance criteria, and structured use cases. |
