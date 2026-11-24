# Inventory and Invoicing System | Compact Edition

**Author** Brandon Narine  
**Date** November 24, 2026  
**Status** Phase 1, Business Analysis and Planning, is complete. The system has yet to be built.

This condensed companion covers the Release 1 analysis through planned delivery. The [full artifact set](README.md#artifact-index) remains the source of truth. All personas and sample data are fictional.

## The problem and the business

A single-store electronics and accessories retailer serves retail and wholesale customers. Sales staff check products and prices in paper and spreadsheet records, prepare invoices manually, and track payments and stock separately. Shelf counts can differ from recorded quantities, invoices vary, and staff have to reconcile records to see what customers owe. The owner lacks a reliable low stock warning and an audit trail for adjustments.

The proposed staff web application combines catalogue, stock, customers, sales, invoices and payments. It checks stock before completion, records stock deductions automatically, generates a consistent printable invoice and makes low stock and outstanding balances visible. The BRD measures median invoice preparation time, completed sale lines sold against unavailable stock, and whether unpaid balances are available and reconcile by the same day's close. The baseline periods and invoice time improvement target still need confirmation. The success conditions are a lower median meeting the agreed target, zero completed lines above available stock during the agreed period, and a same-day view of balances that reconciles to invoices and payments.

## Scope

All 12 features below are in the Release 1 baseline. Must, Should and Could indicate relative build priority. A change to that baseline requires business approval.

| Feature | Release 1 scope | MoSCoW |
| --- | --- | --- |
| Product catalogue | SKU, name, category, unit, retail price, wholesale price, quantity on hand, reorder level, and active status. | Must have |
| Stock receiving | Receive stock against a supplier delivery and record quantity and date. | Must have |
| Stock adjustments | Require a reason code of damage, loss, count correction, return to supplier, or customer return. | Must have |
| Customer records | Store name, type (retail or wholesale), contact details, and balance owed. | Must have |
| Sale creation | Select a customer and add product lines. Validate quantity and price, and block a sale line when quantity on hand is insufficient. | Must have |
| Invoice generation | Generate a unique sequential invoice number, sale lines, totals, payment status, and a printable invoice view. | Must have |
| Payments | Record full and partial payments against an invoice and show payment history per customer. | Must have |
| Receivables list | Show unpaid and partly paid invoices sorted by age. | Must have |
| Low stock list | Show products at or below their reorder level. | Must have |
| Basic reporting | Show sales by period, sales by product, and stock on hand value. | Could have |
| User roles and permissions | Apply permissions for the Owner, Sales Clerk, and Inventory Clerk. | Must have |
| Search and filtering | Support search and filtering on products, customers, sales, and invoices. | Should have |

### Outside Release 1

| Excluded feature | Future release | MoSCoW |
| --- | --- | --- |
| Online store or customer self service portal | Release 3 for customer portal evaluation. Release 4 proposed for portal implementation and an online store, subject to that evaluation and approval. | Will not have this release |
| Card payment processing and payment gateways | Release 4 proposed. | Will not have this release |
| Accounting, payroll, and tax filing | Release 4 proposed. | Will not have this release |
| Barcode scanner and label printer hardware integration | Release 2 includes barcode support. Release 2 proposed for scanner and label printer hardware integration, subject to confirmation of the devices covered. | Will not have this release |
| Multiple store locations and stock transfers between locations | Release 3 includes second location support. Release 3 proposed for stock transfers between locations. | Will not have this release |
| Delivery scheduling and routing | Release 4 proposed. | Will not have this release |
| Supplier purchase orders and automatic reordering | Release 2 includes supplier purchase orders. Release 4 proposed for automatic reordering. | Will not have this release |
| Demand forecasting | Release 3 includes a forecasting report. Release 3 proposed for demand forecasting within that report. | Will not have this release |

## People

| Persona or role | Goal and responsibilities |
| --- | --- |
| Owner and Manager, Alina Veylan | Approves initial prices and price changes, approves higher value adjustments, manages staff access, and reviews low stock, receivables and reports. Needs to see what sells, what is running low and who owes money without reconstructing paper records. |
| Sales Clerk, Devren Marlowe | Looks up products and approved prices, checks availability, maintains Customer records, records sales and full or partial payments, and prints invoices. Needs a fast counter workflow and a clear remaining balance. |
| Inventory Clerk, Maris Fenwick | Counts and receives deliveries, records adjustments with an approved reason and investigates differences in stock. Needs traceable stock movements and a reliable quantity on hand. |
| Customer, Talia Norwyn | Buys retail or wholesale and receives itemized invoices and payment confirmation from staff. Has no Release 1 login. |
| System Administrator role | Held by the Owner and Manager as a permission set for staff accounts and permissions. |

## Business requirements

| ID | Business need | Business reason |
| --- | --- | --- |
| BR-01 | Maintain one product catalogue with a unique SKU for each product, its name, category, unit, retail and wholesale prices, quantity on hand, reorder level, and active status. | Staff need a consistent reference for products, pricing, availability, and replenishment. |
| BR-02 | Record stock received against each supplier delivery, including how much was received and when, and reflect it in stock on hand. | The Inventory Clerk needs to reconcile increases in stock to actual deliveries. |
| BR-03 | Record stock adjustments with a mandatory reason from the approved list of damage, loss, count correction, return to supplier, or customer return. | Staff need to explain discrepancies and distinguish routine corrections from losses. |
| BR-04 | Maintain customer records with customer type, the name and contact details needed for invoicing and contact, and the outstanding balance. | Staff need to serve retail and wholesale customers and identify who owes money using a single record. |
| BR-05 | Record each sale against a customer with itemized products, and validate quantities and prices against the approved pricing for the customer's type. | Customers need accurate charges, and the owner needs consistent pricing. |
| BR-06 | Verify availability before completing each sale and block a line when the requested quantity exceeds stock on hand. | Staff need to avoid recording sales of unavailable stock. |
| BR-07 | Deduct sold quantities automatically when a sale is completed. | Subsequent sales and stock reports need to reflect the units that remain. |
| BR-08 | Give each completed sale a printable, itemized invoice with a unique sequential number, calculated totals, and payment status. | Customers need a consistent record of charges, and staff need to identify each invoice unambiguously. |
| BR-09 | Record full and partial payments against the relevant invoice. | Each payment must be attributable to the debt it settles so staff can resolve disputes. |
| BR-10 | Keep invoice payment status and customer balances current after payments, and make payment history available by customer. | Staff need to confirm what has been paid and what is still owed. |
| BR-11 | Show the owner unpaid and partly paid invoices by age and the amounts still owed on the same business day. | The owner needs to prioritize collection and see current receivables. |
| BR-12 | Identify products at or below their reorder level. | Staff need to see potential shortages early enough to act. |
| BR-13 | Give the owner sales summaries by period and by product. | The owner needs to see when sales occur and which products are selling. |
| BR-14 | Give the owner the value of stock on hand using the approved valuation basis. | The owner needs to understand how much value is held in current inventory. |
| BR-15 | Limit staff activities to the approved responsibilities of the Owner, Sales Clerk, and Inventory Clerk, and let the owner manage staff access. | The business needs to restrict access to records, changes, and approvals to authorized staff. |
| BR-16 | Attribute system activity to individual staff accounts. | The owner needs to identify who performed a transaction or changed a record. |
| BR-17 | Require the Store Owner and Manager to oversee approval of product prices and price changes, restrict who can make changes, and use a separate authorized approver when a second user is available. | The owner needs oversight of price changes to prevent unauthorized discounts and fraud. |
| BR-18 | Require owner approval for stock adjustments above the agreed threshold and use a separate authorized approver when a second user is available. | Higher value stock movements need independent scrutiny to reduce the risk of loss or fraud. |
| BR-19 | Maintain an audit trail of sales, stock adjustments, price changes, payment recording, and failed sign-ins, with the responsible account or sign-in identity and when the event occurred. Protect that history from changes or deletion by regular users. | The owner needs evidence to investigate stock loss, payment disputes, and suspicious access. |
| BR-20 | Enable staff to search and filter products, customers, sales, and invoices within their authorized access. | Staff need to find operational records quickly during routine work and follow-up. |
| BR-21 | Back up business records daily and demonstrate that they can be restored before sign off. | The business needs to recover stock, invoice, and payment records after accidental loss or a system failure. |

## How it works

1. **Counter sale, UC-01.** A Customer requests a product. The Sales Clerk finds it, selects the Customer, sees any outstanding balance and adds one or more lines. The application checks quantities and uses the approved retail or wholesale price for the selected Customer type. A missing Customer, empty sale or invalid line prevents completion.
2. **Availability and exceptions.** Before completion, group requested quantities by SKU and compare them with current stock. If any total exceeds stock, identify the affected lines and let the clerk adjust or remove them. Revalidate using current availability when the clerk retries. If no lines remain, the sale ends without an invoice.
3. **Completion and invoice, UC-02.** Commit the completed sale, one itemized invoice and the stock deduction together. Assign the next unique sequential number, calculate totals from recorded lines and expose a printable view with payment status. Opening an issued invoice again uses its existing number. Incomplete sales do not produce an invoice.
4. **Settlement.** The Sales Clerk records any full or partial payment against the issued invoice. A sale can complete without counter payment, with the invoice unpaid and its full amount added to the Customer's open balance. Posting payment updates payment status and balances. Low stock follows the stock deduction at sale completion. The future-state process diagram's placement of F13 is recorded for reconciliation in Artifact 8. This summary follows Artifact 5 rule 3.
5. **Receive Stock, UC-03.** The Inventory Clerk selects an active product and compares the count with the delivery note. Post a verified positive receipt against the SKU once. Hold a mismatch with the expected count, actual count and note until the clerk confirms the counted quantity. Reject inactive product receipts. Only posting changes stock.
6. **Stock corrections and oversight.** Record a signed quantity change and a reason. A routine valid adjustment posts once. An adjustment above the threshold awaits owner approval and is rechecked before posting. The owner reviews receivables, low stock, sales, stock value and the audit trail using their permitted views.

**ERD entities** Product, Customer, Sale, Sale Item, Invoice, Payment, Stock Adjustment, Supplier Delivery, User.

## User stories

The 24 stories total **91 points**. Estimates are relative. A three-point story uses US-01 as the reference for a bounded workflow with several validations. A story is ready when its persona, BR links, priority, estimate, observable criteria, dependencies and necessary decisions are in place. Completion calls for passing acceptance criteria with fictional records, verifying applicable approvals and audit entries, updating documentation and traceability, and business review.

| ID | Story and acceptance focus | Linked BRs | MoSCoW | Points |
| --- | --- | --- | --- | --- |
| US-01 | Owner and Manager maintains a single catalogue reference for staff. Save an unused SKU with catalogue details, reject a duplicate while preserving its existing record, and show revised reorder level and active status after edits. Price approval follows US-02 and opening stock follows physical reconciliation. | BR-01 | Must have | 3 |
| US-02 | Owner and Manager controls which prices staff can charge. Hold initial and changed prices until approval, keep a prior approved price effective while a change is pending, deny clerk access, and attribute the approved change and approval. Require independent approval when another authorized approver is available. | BR-01, BR-17, BR-19 | Must have | 5 |
| US-03 | Inventory Clerk reconciles stock gains to deliveries. Record an active product's SKU, quantity and date, increase stock once after verification, hold a counted mismatch before posting, and reject inactive products without a stock change. Grouping multiple products on one delivery remains open. | BR-02 | Must have | 3 |
| US-04 | Inventory Clerk explains stock differences with a recognized reason code. An otherwise valid routine adjustment updates quantity on hand once and identifies the account, reason, date and time. Reject a missing or unrecognized reason without changing stock. Route above-threshold adjustments to US-05. | BR-03, BR-18, BR-19 | Must have | 3 |
| US-05 | Owner and Manager checks higher value changes before posting. Hold an adjustment above G$10,000, block self-approval when another authorized approver is available and log who recorded and approved it. Exactly G$10,000 is within the routine limit. Recheck stock before posting. | BR-18, BR-19 | Must have | 5 |
| US-06 | Sales Clerk maintains Customer records for invoicing and follow-up. Save name, retail or wholesale type and necessary contacts, make the Customer available for sale selection, and preserve the calculated outstanding balance when contacts change. Opening balances require reconciliation to unpaid invoices and payments. | BR-04 | Must have | 2 |
| US-07 | Sales Clerk selects a Customer and enters itemized products, quantities and prices. Completion requires the Customer, at least one line and valid positive quantities. Show an existing balance at selection while allowing a sale that meets the other checks. Save the validated lines against that Customer. | BR-04, BR-05 | Must have | 5 |
| US-08 | Sales Clerk charges the approved price matching the Customer's type. Select approved retail or wholesale price from the Product, using one approved wholesale price per Product under the proposed default. Block completion until any missing or different price is corrected. | BR-05, BR-17 | Must have | 3 |
| US-09 | Sales Clerk validates availability before completing a sale. Allow the last unit, block any SKU whose total across lines exceeds stock, and recheck current quantities after correction or removal. If all lines are removed, end without an invoice. | BR-06 | Must have | 5 |
| US-10 | Inventory Clerk relies on automatic stock deduction after a completed sale. Deduct the total sold per SKU once at completion, including when the Customer pays nothing at the counter. Further payments leave stock unchanged. | BR-07 | Must have | 5 |
| US-11 | Sales Clerk issues one numbered invoice from valid completed Sale Items. Use the next sequential number and system-calculated totals and show unpaid before any payment. Reject incomplete invoicing and return the original invoice on retry. The draft omits a separate tax line pending confirmation. | BR-08 | Must have | 5 |
| US-12 | Sales Clerk provides a paper copy of an issued invoice. Show the original number, itemized lines, calculated totals and payment status at print time, including any partial or full payment recorded since issue. Incomplete sales have no invoice to print. | BR-08, BR-10 | Must have | 2 |
| US-13 | Sales Clerk records full and partial payments against the invoice they settle. Capture the invoice number, positive amount, recorder and timestamp and audit the event. Validate the Invoice and apply the current balance limit in rule 5. Rejected submissions leave amounts owed unchanged. | BR-09, BR-19 | Must have | 5 |
| US-14 | Sales Clerk sees current payment status and the balance owed. An issued invoice with no payment is unpaid. A partial payment reduces the amount owed and leaves the invoice partly paid. Full settlement makes it paid, while other open invoices remain in the Customer balance. | BR-04, BR-10 | Must have | 3 |
| US-15 | Sales Clerk reviews payment history by Customer and linked Invoice. Show date, time and amount, exclude other Customers' payments and retain paid invoices and their payments throughout Release 1. Confirm settlement through the updated printed Invoice and recorded payment history under the draft default. | BR-09, BR-10 | Must have | 3 |
| US-16 | Owner and Manager reviews open invoices by age for collection. See current amounts owed on unpaid and partly paid invoices, including the reduced amount after a partial payment on the same day. A settled invoice leaves the list. Rule 6 specifies the order. | BR-11 | Must have | 3 |
| US-17 | Owner and Manager uses the Low stock list to identify shortages. Include stock below and exactly at the confirmed reorder level. A sale can bring a product onto the list, and a receipt that raises it above the level removes it. | BR-12 | Must have | 2 |
| US-18 | Owner and Manager reviews sales by period and product. Reconcile period totals to completed sales within the selected dates, and reconcile each product's quantities and sales amounts to completed Sale Items. | BR-13 | Could have | 3 |
| US-19 | Owner and Manager checks current stock on hand value. Use reconciled stock quantities and the confirmed valuation basis and inputs, show the basis and recalculate after stock changes. Confirm the basis before the story is ready. | BR-14 | Could have | 3 |
| US-20 | Owner and Manager enforces approved access by staff role. Allow approved functions under individual accounts, deny a Sales Clerk direct access to the price-change function, and require sign-in for direct Invoice access when signed out. The owner must confirm the permissions. | BR-15 | Must have | 5 |
| US-21 | Owner and Manager manages individual accounts and attribution. Give staff distinct identifiers even within one role, tie authorized transactions to the responsible account and apply access changes on subsequent requests while preserving historical attribution. | BR-15, BR-16 | Must have | 5 |
| US-22 | Owner and Manager can reconstruct disputed activity using protected audit entries. Record completed sales, adjustments, approved price changes and payments with responsible account and time, and failed sign-ins with attempted identity and time. Enforce protection against edit or deletion. | BR-16, BR-19 | Must have | 5 |
| US-23 | Sales Clerk searches and filters product, customer, sale and invoice records. Return only records within authorized access that meet both the search and applied filters. Access follows US-20. | BR-20 | Should have | 3 |
| US-24 | Owner and Manager verifies recoverability. Record daily backup completion and demonstrate a restore of fictional business records before sign off. Reconcile restored stock, Customer records, sales, invoices, payments and balances to the backup. Any failed or discrepant restore leaves the criterion open. | BR-21 | Must have | 5 |

## Key business rules

1. **Invoice numbering.** Assign the next unique sequential Invoice number at successful invoice generation. An assigned number is permanent and never reused. An abandoned or rejected sale consumes no number. Concurrent invoice generation must preserve the same sequence. Reopening or reprinting an issued Invoice keeps its original number.
2. **Price selection.** Customer type selects the retail price or wholesale price from the Product record at sale completion. One approved wholesale price applies to all wholesale Customers. Proposed prices become usable through FR-05. A completed Sale Item keeps its recorded unit price through later Product price changes. An outstanding balance is displayed at Customer selection and does not block an otherwise valid sale.
3. **Stock deduction.** At sale completion, check combined quantities by product SKU against current stock and commit the completed Sale, Invoice and deductions together. Concurrent stock changes must preserve that check. An unsuccessful completion leaves stock unchanged and issues no Invoice. Deduct once even when the Customer pays nothing at the counter. Later payments affect settlement only.
4. **Invoice calculation and delivery.** The calculated total is the sum of completed Sale Item quantities multiplied by their recorded unit prices. The Release 1 draft uses the approved default of omitting a separate tax line pending confirmation. Use the printed Invoice for delivery and the updated printed Invoice and payment history for payment confirmation. Retain issued Invoices and Payments throughout Release 1, with automatic deletion deferred pending a retention decision.
5. **Payment and balances.** Invoice amount owed equals calculated total less the sum of recorded payments against that Invoice. Payment status is unpaid when no payment has been recorded, partly paid when payments leave a positive balance, and paid when the balance reaches zero. The Customer's balance owed equals the sum of that Customer's open Invoice balances. A payment must be positive and is limited to the current Invoice balance. Reject any greater submission, display the balance owed and let the clerk enter a corrected amount when a positive balance remains. Record no payment on rejection and create no customer credit balance. The balance check and payment posting use the same current balance, including when payments are submitted concurrently.
6. **Receivables ordering.** The Receivables list shows only Invoices with a positive balance. Order by issue date from oldest to newest, then by Invoice number ascending for the same issue date. An Invoice leaves the list when its balance reaches zero.
7. **Receiving mismatch.** Record a mismatch with the expected quantity from the delivery note, counted quantity and a note. Hold posting until the Inventory Clerk confirms the counted quantity. For a match, submitting the verified quantity constitutes confirmation. The confirmed positive count becomes quantity received when posting. A held receipt has no stock effect.
8. **Stock adjustment valuation.** For the approval check, multiply the magnitude of quantity change by the Product's current approved wholesale price at submission. The magnitude applies to increases and decreases alike. Above G$10,000 requires Owner and Manager approval before posting. Exactly G$10,000 is within the routine posting limit. This is an invented analysis threshold. Hold posting if the required valuation input is unavailable.
9. **Approval independence.** When a second authorized approver is available, require that person to approve a price proposal or a stock adjustment that requires approval. When no second authorized approver exists, the Owner and Manager may approve their own proposal or adjustment and the audit trail identifies the action as self-approved. The approver must have the Owner role in either case.
10. **Low stock.** A Product appears on the Low stock list when quantity on hand is at or below reorder level and leaves when quantity on hand exceeds reorder level. Use the current recorded stock after each posted movement.
11. **Stock on hand value.** The valuation basis is an explicit report setting the Owner and Manager must confirm. The report displays the confirmed basis alongside the value. The BR-14 valuation basis and resulting valuation inputs remain open for business confirmation.

### Functional requirement reference

The planned rules are implemented through the following requirements. The case IDs refer to the designed tests in Artifact 6.

| ID | Planned requirement | Stories | Designed cases |
| --- | --- | --- | --- |
| FR-01 | Save a Product with an unused SKU and valid catalogue details, showing all fields defined in Product Record. | US-01 | TC-01 |
| FR-02 | Reject creation of a Product whose SKU already exists, preserving the existing Product. | US-01 | TC-02 |
| FR-03 | Save edits to catalogue details, including reorder level and active status, under the field rules in Product Record. | US-01 | TC-03 |
| FR-04 | Keep a proposed retail price or wholesale price unavailable for sale pricing until approved, with any previously approved price remaining in effect. | US-02 | TC-04 |
| FR-05 | Make a proposed retail price or wholesale price effective for new sales when an Owner and Manager approves it under SEC-10. | US-02 | TC-04 |
| FR-06 | Post a confirmed receipt for an active Product once, recording the Supplier Delivery and increasing quantity on hand by quantity received in the same posting. | US-03 | TC-05, TC-06, TC-07 |
| FR-07 | Hold a receiving mismatch as specified in rule 7, with no effect on quantity on hand before confirmation. | US-03 | TC-06 |
| FR-08 | Reject receipt posting for an inactive Product without changing quantity on hand. | US-03 | TC-07 |
| FR-09 | Reject a stock adjustment with a missing or unrecognized reason code without changing quantity on hand. | US-04 | TC-08 |
| FR-10 | Post an otherwise valid stock adjustment at or below the threshold when submitted, recording its Stock Adjustment fields and applying the quantity change once. | US-04, US-05 | TC-09, TC-13 |
| FR-11 | Hold an otherwise valid stock adjustment above the threshold for Owner and Manager approval without changing quantity on hand. | US-05 | TC-10, TC-12, TC-13 |
| FR-12 | Post a held stock adjustment once following Owner and Manager approval under SEC-10, provided the resulting quantity on hand remains nonnegative. | US-05 | TC-10, TC-12 |
| FR-13 | Reject posting a stock adjustment that would reduce quantity on hand below zero. | US-04 | TC-11, TC-12 |
| FR-14 | Make each valid newly saved Customer available for sale selection with the Customer Record fields. | US-06 | TC-14 |
| FR-15 | Save valid edits to a Customer's name, type and contact details without changing the calculated balance owed. | US-06 | TC-15 |
| FR-16 | Save a completed sale against the selected Customer with its validated Sale Items. | US-07 | TC-16, TC-17 |
| FR-17 | Block completion of a sale with no selected Customer, no product lines, or any line with an invalid quantity. | US-07 | TC-18 |
| FR-18 | Show the existing balance owed on Customer selection, with that balance having no effect on whether an otherwise valid sale may complete. | US-07 | TC-16 |
| FR-19 | Supply each line's unit price from the approved Product price for the selected Customer type. | US-08 | TC-16, TC-17 |
| FR-20 | Block sale completion if a line lacks the applicable approved price or its unit price differs from the applicable approved price at completion. | US-08 | TC-19 |
| FR-21 | Preserve the unit price on each completed Sale Item when the Product price later changes. | US-08, US-11 | TC-20 |
| FR-22 | Block sale completion and identify the affected lines if the total requested for any product SKU exceeds its current quantity on hand, checking again when completion is retried. | US-09 | TC-21, TC-22 |
| FR-23 | Deduct the sold quantity per product SKU exactly once at sale completion, including completion without payment. | US-10 | TC-16, TC-21, TC-22 |
| FR-24 | Issue one itemized Invoice with the next unique sequential Invoice number when a sale completes with valid Sale Items. | US-11 | TC-23, TC-24, TC-25 |
| FR-25 | Calculate the Invoice total from the completed Sale Items under rule 4. | US-11 | TC-23 |
| FR-26 | Reject invoice generation for an incomplete sale or a sale without completed Sale Items. | US-11, US-12 | TC-24 |
| FR-27 | Return the existing Invoice and its original Invoice number when invoicing is requested again for the same completed sale. | US-11 | TC-26 |
| FR-28 | Provide a printable view of an issued Invoice with the fields specified in Invoice View and Print, including current payment status at print time. | US-12, US-15 | TC-26 |
| FR-29 | Record a valid positive payment up to the current balance against its issued Invoice, with the fields defined in Invoice View and Print. | US-13 | TC-28, TC-29, TC-32 |
| FR-30 | Reject a payment with a missing or invalid Invoice number, a zero or negative amount, or an amount exceeding the current balance, applying rule 5. | US-13 | TC-30, TC-31, TC-32 |
| FR-31 | Recalculate the amount owed on an Invoice from its total and recorded payments, including when first issued. | US-14 | TC-27, TC-28, TC-29, TC-32 |
| FR-32 | Derive the Invoice payment status from the recorded payments and amount owed under rule 5. | US-14 | TC-27, TC-28, TC-29, TC-32 |
| FR-33 | Derive the Customer's balance owed from its open invoice balances under rule 5. | US-14 | TC-27, TC-28, TC-29 |
| FR-34 | Show payment history for the selected Customer only, including Payment records for settled Invoices. | US-15 | TC-33 |
| FR-35 | Keep issued Invoices and Payment records available throughout Release 1, including after settlement. | US-15 | TC-33 |
| FR-36 | Show only unpaid and partly paid Invoices on the Receivables list, with current amounts owed after each recorded payment and ordering per rule 6. | US-16 | TC-34 |
| FR-37 | Include a Product on the Low stock list when its current quantity on hand is at or below its reorder level and remove it when quantity on hand rises above that level. | US-17 | TC-35 |
| FR-38 | Show sales by period as the total of completed sales within the selected period. | US-18 | TC-36 |
| FR-39 | Show sales by product as the sum of quantity and quantity multiplied by unit price for completed Sale Items, grouped by product SKU. | US-18 | TC-37 |
| FR-40 | Calculate stock on hand value from current quantities and the basis identified in the Owner and Manager-confirmed valuation setting, displaying that basis with the report and withholding the value until the setting is confirmed and the required inputs are available. | US-19 | TC-38 |
| FR-41 | Return only authorized Products that match the Product List search and all applied filters. | US-23 | TC-39 |
| FR-42 | Return only authorized Customers that match the Customer List search and all applied filters. | US-23 | TC-40 |
| FR-43 | Return only authorized Sales that match the Sales List search and all applied filters. | US-23 | TC-41 |
| FR-44 | Return only authorized Invoices that match the Invoice List search and all applied filters. | US-23 | TC-42 |

## Roles and permissions

Owner and Manager holds the Owner role and the System Administrator permission set. The Customer receives printed records from the Sales Clerk and has no system login.

| Function | Owner and Manager | Sales Clerk | Inventory Clerk |
| --- | --- | --- | --- |
| Sign In, open Dashboard and sign out | Allowed | Allowed | Allowed |
| View, search and filter Product List for catalogue details and stock | Allowed | Allowed | Allowed |
| View approved retail price and wholesale price on Product List | Allowed | Allowed | Not allowed |
| Create or edit Product Record catalogue details and propose prices | Allowed | Not allowed | Not allowed |
| Approve initial prices and price changes under rule 9 | Allowed | Not allowed | Not allowed |
| Record, confirm, post and review stock receipts | Not allowed | Not allowed | Allowed |
| Record stock adjustments | Allowed in the single-authorized-user case in rule 9 | Not allowed | Allowed |
| Review stock adjustment records | Allowed | Not allowed | Allowed |
| Approve stock adjustments above G$10,000 under rules 8 and 9 | Allowed | Not allowed | Not allowed |
| Search or filter Customer List and select a Customer | Not allowed | Allowed | Not allowed |
| Create and edit Customer Record, view balance owed and payment history | Not allowed | Allowed | Not allowed |
| Search and filter Sales List and Invoice List | Not allowed | Allowed | Not allowed |
| Create and complete sales, including stock validation and invoice generation | Not allowed | Allowed | Not allowed |
| Open and print an issued Invoice | Not allowed | Allowed | Not allowed |
| Record a payment | Not allowed | Allowed | Not allowed |
| View Receivables List | Allowed | Not allowed | Not allowed |
| View Low Stock List | Allowed | Not allowed | Not allowed |
| Run Reports and confirm the valuation basis setting | Allowed | Not allowed | Not allowed |
| Manage User Accounts and set the inactivity timeout | Allowed | Not allowed | Not allowed |
| View Audit Trail | Allowed | Not allowed | Not allowed |
| Edit or delete audit entries | Not allowed | Not allowed | Not allowed |

## Security in brief

The application will use **OWASP ASVS Level 1** as its security baseline because it handles Customer names, contact details and invoiced, paid and owed amounts. The BRD uses **OWASP Top 10** as the risk reference. Its directly applicable concerns include broken access control, injection, identification and authentication failures, and security logging and monitoring failures. Payment recording only records settlements against Invoices. Card payment processing and storage are outside scope, so PCI DSS does not apply.

**Chosen values** Minimum password length of 15 characters. Lock sign-in for 15 minutes after 5 consecutive failed attempts. Initial inactivity timeout of 15 minutes, configurable by the Owner and Manager.

| ID | Planned control | Designed cases |
| --- | --- | --- |
| SEC-01 | Assign each staff member one individual User account with a unique User ID and sign-in identifier. Shared accounts are prohibited. Start a staff session only after validating the credentials of an enabled account under SEC-05. Attribute authorized sales, stock adjustments and payments to the authenticated account. | TC-43 |
| SEC-02 | Allow only the Owner and Manager to manage individual staff accounts and their assigned roles, using the permissions matrix. Changes to account access take effect on subsequent requests and preserve original attribution for past records. | TC-44 |
| SEC-03 | Require a minimum of 15 characters whenever a staff password is set or changed. | TC-45 |
| SEC-04 | Store passwords only as salted hashes using a password hashing function. Passwords must never be stored as plaintext or in reversible form. | TC-46 |
| SEC-05 | Lock sign-in for an account for 15 minutes after 5 consecutive failed attempts. A successful sign-in or expiry of the lockout resets the failed attempt count. Show the same sign-in failure message for invalid credentials, unknown identifiers and temporarily locked accounts. | TC-47 |
| SEC-06 | Enforce current role permissions on the server for every protected request, including function, record and field access, direct addresses and searches. Deny unauthorized requests without exposing protected data or changing records. | TC-48, TC-49 |
| SEC-07 | Redirect a signed-out staff user requesting a protected screen, including a direct Invoice address, to Sign In before returning any protected details. Reject unauthenticated protected requests. | TC-50 |
| SEC-08 | Return application content and accept credentials or other application input only over HTTPS. | TC-51 |
| SEC-09 | End staff sessions after the inactivity period set by the Owner and Manager, initially 15 minutes. Reject requests using an expired session and require fresh sign-in. | TC-52 |
| SEC-10 | Enforce rule 9 at approval time for initial prices, price changes and stock adjustments above the threshold. Check the approver's current authorization, whether the approver is the proposer or recorder, and whether a second authorized approver is available. The audit trail identifies the proposer or recorder and approver, including permitted self-approval. | TC-53, TC-54 |
| SEC-11 | End the authenticated session on sign-out so that subsequent requests using that session cannot reach protected data or actions. | TC-55 |
| SEC-12 | Capture the events and required metadata in Audit trail requirements, including synchronized UTC timestamps, authentication method for sign-ins, and relevant before and after values. Record an associated audit entry for every completed audited business action. Exclude credential values, session secrets and card data. | TC-56, TC-57 |
| SEC-13 | Allow only the Owner and Manager to view audit entries. | TC-58 |
| SEC-14 | Prevent every system user, including the Owner and Manager, from editing or deleting audit entries. | TC-59 |
| SEC-15 | Validate form inputs and search criteria on the server. Reject negative quantities and monetary amounts and require positive values for sale quantity, quantity received, proposed selling prices and payment amount. An adjustment's signed quantity change is a nonzero difference, subject to FR-13. | TC-60 |
| SEC-16 | Limit collection of customer data to the name, type and contact details needed for invoicing and contact. | TC-61 |
| SEC-17 | Prevent card data from being stored in any field. Staff must never enter card numbers or any other card data in free text. Verify this business process rule during UAT before sign off. | TC-62 |
| SEC-18 | Calculate Invoice totals from validated Sale Items and reject a manually supplied Invoice total. | TC-63 |
| SEC-19 | Complete a backup of system data each day, with the outcome recorded so that the Owner and Manager can verify daily completion. | TC-64 |
| SEC-20 | Support a restore from backup for verification before Release 1 sign off. Use fictional sample data and reconcile restored stock, Customer records, sales, invoices, payments and balances to the backup. Keep the restore criterion open after any failed restore or discrepancy until a successful restore and reconciliation are demonstrated. | TC-65 |
| SEC-21 | Protect quantity on hand from going below zero on sale completion, including during concurrent sales or stock changes. | TC-66 |

### Selected Level 2 controls

- **Audit event detail and protection, SEC-12 to SEC-14.** Use structured event metadata, synchronized timestamps, authentication and denied-access event capture, and protected audit records. These controls let the Owner and Manager reconstruct disputed payments, unauthorized access and unexplained stock movements. The ASVS 5.0.0 references are 16.2.1, 16.2.2, 16.3.1, 16.3.2 and 16.4.2. [OWASP ASVS V16](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x25-V16-Security-Logging-and-Error-Handling.md)
- **Separation of duties on prices and stock adjustments, SEC-06 and SEC-10.** Document and enforce who can write approved prices and approval state, and enforce the adjustment approval threshold as a business logic limit. These selected Level 2 controls are 8.1.2, 8.2.3 and 2.3.2. Rule 9 adds the project's independent-approval rule and the audited sole-authorized-approver exception to reduce unauthorized discounts and unexplained stock loss. ASVS 5.0.0 lists its general high-value multi-user approval requirement, 2.3.5, at Level 3. [OWASP ASVS V8](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x17-V8-Authorization.md), [OWASP ASVS V2](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x11-V2-Validation-and-Business-Logic.md)

## Testing and acceptance in brief

The 66 designed system test cases cover every functional and security requirement. The set combines positive, negative, boundary and concurrency checks using fictional fixtures and observable expected results. The nine UAT scenarios exercise trading and control workflows using individual accounts, including role access and restoration.

**Boundary and rejection checks** Sell the last unit and reject one unit above stock. Reject a duplicate SKU and a missing adjustment reason. Post at exactly G$10,000 and hold above it. Include stock at the reorder level. Verify approved wholesale pricing, partial settlement, payment equal to the remaining balance, and rejection at one cent above it. Check the password minimum, lockout, session expiry, and concurrent stock, payment and invoice numbering boundaries.

| ID | Scenario |
| --- | --- |
| UAT-01 | Run a normal trading day |
| UAT-02 | Return an item to stock |
| UAT-03 | Approve a higher value stock adjustment |
| UAT-04 | Confirm a delivery that comes up short |
| UAT-05 | Prepare the catalogue and approve a price change |
| UAT-06 | Reconcile receivables, stock and sales |
| UAT-07 | Onboard a new retail Customer |
| UAT-08 | Role access check |
| UAT-09 | Backup restore check |

- **Entry.** Complete system testing with no critical defects open. Prepare UAT with Artifact 6 fictional records, an isolated restore destination, individual role accounts and briefed participants. Confirm a second authorized Owner and Manager for UAT-05 and an authorized restore operator for UAT-09.
- **Execution.** Begin with the role access check and verify both allowed and unavailable functions. Record scenario outcomes and defects. Restore the sample backup in UAT and reconcile stock, Customer records, sales, invoices, payments and balances against the backup.
- **Exit.** Complete all scenarios and clean retests, pass both operational checks and close all critical and major defects. The owner accepts any remaining minor defects in writing.
- **Sign off.** The owner settles or formally defers open decisions, records the affected scope, verification still due and revisit timing for any deferral, and signs the Release 1 acceptance record.

## Traceability result

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

Artifact 8 found no BRs, stories, functional or security requirements, or test cases without their required links. The figures describe designed coverage. Dedicated system tests check the password boundary, lockout, signed-out access, idle expiry and access after sign-out. UAT-08 verifies role permissions. UAT-01 and UAT-07 check staff handling of card data, while TC-62 covers storage verification and depends on the free-text handling decision. Numeric valuation verification for FR-40 depends on confirmation of the basis and its inputs.

## Roadmap in brief

1. **Business Analysis and Planning.** Completed the analysis, requirements, process and acceptance artifacts, coverage check and roadmap.
2. **Design.** Plan screens and interfaces, develop the data design from the ERD outline, finalize the draw.io diagrams and confirm BR-14 and US-20 at the design gate.
3. **Build.** Establish individual accounts, roles and audit foundations before implementing catalogue, stock, customers, sales, invoicing, payments and reporting. Add backup and restore capability.
4. **System Testing.** Execute Artifact 6, fix defects and retest, then confirm readiness for UAT.
5. **UAT.** Execute Artifact 7, including role access and a reconciled restore of fictional data, and obtain owner sign off.
6. **Delivery and Go Live.** Reconcile and load opening stock and balances, set up accounts, train by role, reconcile the parallel invoicing run, go live and provide short post go live support.

**Release 2** Supplier purchase orders, barcode support and customer statements, with scanner and label printer hardware integration proposed.  
**Release 3** Second location support, a forecasting report and customer portal evaluation, with stock transfers and demand forecasting within the report proposed.

## Open items

- **BR-14 valuation basis.** Confirm the stock on hand valuation basis and required inputs for FR-40, TC-38 and UAT-06. Rule 8 already defines the separate adjustment valuation used for approval.
- **US-20 permissions.** Confirm role access, price approval permissions and how owner oversight works where clerks have access to individual records and stock receipt workflows. Confirm whether clerk-assisted record review meets the owner's oversight needs.
- **Seven scope defaults for confirmation.** One approved wholesale price per product for all wholesale Customers, Invoice and Payment retention throughout Release 1 with automatic deletion deferred, existing balance visible while an otherwise valid sale proceeds, Owner and Manager approval above G$10,000 with a separate authorized approver when available, omission of a separate tax line pending confirmation, printed invoices, and updated printed invoices with payment history for payment confirmation pending a decision on separate receipts.

The [27-item register in Artifact 8](08-traceability-matrix.md#consolidated-open-items-register) records each remaining decision or confirmation and who owns it.
