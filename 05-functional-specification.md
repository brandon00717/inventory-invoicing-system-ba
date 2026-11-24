# Functional Specification

**Project** Inventory and Invoicing System  
**Author** Brandon Narine  
**Date** November 24, 2026  
**Status** Draft for Phase 1 business analysis. The system has yet to be built.

## Functional requirements

Each requirement applies to Release 1. The permissions matrix governs staff access. Future releases cover supplier purchase orders and barcode support in Release 2, and second location support, forecasting and customer portal evaluation in Release 3. Artifact 2 proposes scanner and label printer hardware integration for Release 2, stock transfers for Release 3, and an online store and customer self service implementation subject to portal evaluation, card processing and gateways, accounting, payroll, tax filing, delivery scheduling and routing, and automatic reordering for Release 4. [Artifact 2 future-release assignments](02-stakeholder-and-scope-analysis.md#out-of-scope-for-release-1)

### Product catalogue

| ID | The system shall | Stories | Business requirements |
| --- | --- | --- | --- |
| FR-01 | Save a Product with an unused SKU and valid catalogue details, showing all fields defined in Product Record. | US-01 | BR-01 |
| FR-02 | Reject creation of a Product whose SKU already exists, preserving the existing Product. | US-01 | BR-01 |
| FR-03 | Save edits to catalogue details, including reorder level and active status, under the field rules in Product Record. | US-01 | BR-01 |
| FR-04 | Keep a proposed retail price or wholesale price unavailable for sale pricing until approved, with any previously approved price remaining in effect. | US-02 | BR-01, BR-17 |
| FR-05 | Make a proposed retail price or wholesale price effective for new sales when an Owner and Manager approves it under SEC-10. | US-02 | BR-01, BR-17 |

### Stock receiving

| ID | The system shall | Stories | Business requirements |
| --- | --- | --- | --- |
| FR-06 | Post a confirmed receipt for an active Product once, recording the Supplier Delivery and increasing quantity on hand by quantity received in the same posting. | US-03 | BR-02 |
| FR-07 | Hold a receiving mismatch as specified in rule 7, with no effect on quantity on hand before confirmation. | US-03 | BR-02 |
| FR-08 | Reject receipt posting for an inactive Product without changing quantity on hand. | US-03 | BR-02 |

### Stock adjustments

| ID | The system shall | Stories | Business requirements |
| --- | --- | --- | --- |
| FR-09 | Reject a stock adjustment with a missing or unrecognized reason code without changing quantity on hand. | US-04 | BR-03 |
| FR-10 | Post an otherwise valid stock adjustment at or below the threshold when submitted, recording its Stock Adjustment fields and applying the quantity change once. | US-04, US-05 | BR-03, BR-18 |
| FR-11 | Hold an otherwise valid stock adjustment above the threshold for Owner and Manager approval without changing quantity on hand. | US-05 | BR-18 |
| FR-12 | Post a held stock adjustment once following Owner and Manager approval under SEC-10, provided the resulting quantity on hand remains nonnegative. | US-05 | BR-18 |
| FR-13 | Reject posting a stock adjustment that would reduce quantity on hand below zero. | US-04 | BR-03 |

### Customer records

| ID | The system shall | Stories | Business requirements |
| --- | --- | --- | --- |
| FR-14 | Make each valid newly saved Customer available for sale selection with the Customer Record fields. | US-06 | BR-04 |
| FR-15 | Save valid edits to a Customer's name, type and contact details without changing the calculated balance owed. | US-06 | BR-04 |

### Sale creation

| ID | The system shall | Stories | Business requirements |
| --- | --- | --- | --- |
| FR-16 | Save a completed sale against the selected Customer with its validated Sale Items. | US-07 | BR-04, BR-05 |
| FR-17 | Block completion of a sale with no selected Customer, no product lines, or any line with an invalid quantity. | US-07 | BR-05 |
| FR-18 | Show the existing balance owed on Customer selection, with that balance having no effect on whether an otherwise valid sale may complete. | US-07 | BR-04, BR-05 |
| FR-19 | Supply each line's unit price from the approved Product price for the selected Customer type. | US-08 | BR-05, BR-17 |
| FR-20 | Block sale completion if a line lacks the applicable approved price or its unit price differs from the applicable approved price at completion. | US-08 | BR-05, BR-17 |
| FR-21 | Preserve the unit price on each completed Sale Item when the Product price later changes. | US-08, US-11 | BR-05, BR-08, BR-17 |
| FR-22 | Block sale completion and identify the affected lines if the total requested for any product SKU exceeds its current quantity on hand, checking again when completion is retried. | US-09 | BR-06 |
| FR-23 | Deduct the sold quantity per product SKU exactly once at sale completion, including completion without payment. | US-10 | BR-07 |

### Invoice generation

| ID | The system shall | Stories | Business requirements |
| --- | --- | --- | --- |
| FR-24 | Issue one itemized Invoice with the next unique sequential Invoice number when a sale completes with valid Sale Items. | US-11 | BR-08 |
| FR-25 | Calculate the Invoice total from the completed Sale Items under rule 4. | US-11 | BR-08 |
| FR-26 | Reject invoice generation for an incomplete sale or a sale without completed Sale Items. | US-11, US-12 | BR-08 |
| FR-27 | Return the existing Invoice and its original Invoice number when invoicing is requested again for the same completed sale. | US-11 | BR-08 |
| FR-28 | Provide a printable view of an issued Invoice with the fields specified in Invoice View and Print, including current payment status at print time. | US-12, US-15 | BR-08, BR-10 |

### Payments and balances

| ID | The system shall | Stories | Business requirements |
| --- | --- | --- | --- |
| FR-29 | Record a valid positive payment up to the current balance against its issued Invoice, with the fields defined in Invoice View and Print. | US-13 | BR-09 |
| FR-30 | Reject a payment with a missing or invalid Invoice number, a zero or negative amount, or an amount exceeding the current balance, applying rule 5. | US-13 | BR-09 |
| FR-31 | Recalculate the amount owed on an Invoice from its total and recorded payments, including when first issued. | US-14 | BR-10 |
| FR-32 | Derive the Invoice payment status from the recorded payments and amount owed under rule 5. | US-14 | BR-10 |
| FR-33 | Derive the Customer's balance owed from its open invoice balances under rule 5. | US-14 | BR-04, BR-10 |
| FR-34 | Show payment history for the selected Customer only, including Payment records for settled Invoices. | US-15 | BR-09, BR-10 |
| FR-35 | Keep issued Invoices and Payment records available throughout Release 1, including after settlement. | US-15 | BR-09, BR-10 |

### Receivables

| ID | The system shall | Stories | Business requirements |
| --- | --- | --- | --- |
| FR-36 | Show only unpaid and partly paid Invoices on the Receivables list, with current amounts owed after each recorded payment and ordering per rule 6. | US-16 | BR-11 |

### Low stock

| ID | The system shall | Stories | Business requirements |
| --- | --- | --- | --- |
| FR-37 | Include a Product on the Low stock list when its current quantity on hand is at or below its reorder level and remove it when quantity on hand rises above that level. | US-17 | BR-12 |

### Reporting

| ID | The system shall | Stories | Business requirements |
| --- | --- | --- | --- |
| FR-38 | Show sales by period as the total of completed sales within the selected period. | US-18 | BR-13 |
| FR-39 | Show sales by product as the sum of quantity and quantity multiplied by unit price for completed Sale Items, grouped by product SKU. | US-18 | BR-13 |
| FR-40 | Calculate stock on hand value from current quantities and the basis identified in the Owner and Manager-confirmed valuation setting, displaying that basis with the report and withholding the value until the setting is confirmed and the required inputs are available. | US-19 | BR-14 |

### Search and filtering

| ID | The system shall | Stories | Business requirements |
| --- | --- | --- | --- |
| FR-41 | Return only authorized Products that match the Product List search and all applied filters. | US-23 | BR-20 |
| FR-42 | Return only authorized Customers that match the Customer List search and all applied filters. | US-23 | BR-20 |
| FR-43 | Return only authorized Sales that match the Sales List search and all applied filters. | US-23 | BR-20 |
| FR-44 | Return only authorized Invoices that match the Invoice List search and all applied filters. | US-23 | BR-20 |

## Screen specifications

| Screen | Primary users | Purpose |
| --- | --- | --- |
| Sign In | Owner and Manager, Sales Clerk, Inventory Clerk | Start an individual staff session. |
| Dashboard | Owner and Manager, Sales Clerk, Inventory Clerk | Open functions available to the signed-in role. |
| Product List | Owner and Manager, Sales Clerk, Inventory Clerk | Find products, prices and available stock within role permissions. |
| Product Record | Owner and Manager | Create and edit catalogue records and review proposed prices. |
| Stock Receiving | Inventory Clerk | Record confirmed receipts and hold or resolve quantity mismatches. |
| Stock Adjustment | Inventory Clerk, Owner and Manager | Record and investigate adjustments and approve higher value adjustments. |
| Customer List | Sales Clerk | Find Customers for counter service and follow-up. |
| Customer Record | Sales Clerk | Create and edit Customer details and review balance owed and payment history. |
| Sales List | Sales Clerk | Find recorded sales and open their invoices. |
| Create Sale | Sales Clerk | Complete an itemized sale and obtain its invoice. |
| Invoice List | Sales Clerk | Find issued Invoices and their payment status. |
| Invoice View and Print | Sales Clerk | Review and print an issued Invoice and record payments against it. |
| Receivables List | Owner and Manager | Review outstanding Invoices by age. |
| Low Stock List | Owner and Manager | Review Products at or below their reorder level. |
| Reports | Owner and Manager | Run Basic reporting. |
| User Accounts | Owner and Manager | Manage individual staff access. |
| Audit Trail View | Owner and Manager | Review protected operational and security events. |

System-set values are read only. Required output values are populated by the system. Optional search fields and filters impose no condition when blank. Identifier lookups must resolve within the user's authorized records.

### Sign In

Sign staff in with their individual credentials.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| sign-in identifier | Staff login | Yes | Matches an individual User account. |
| Password | Masked credential input | Yes | Checked under SEC-03 to SEC-05. |

**Actions** Sign in.

### Dashboard

Provide an entry point to role-appropriate functions.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| staff name | Signed-in staff member | Yes, system set | From current User. |
| role | Current User role | Yes, system set | From current permissions. |

**Actions** Open functions allowed by the matrix. Sign out.

### Product List

Find catalogue records and check prices or stock.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| SKU or name search | Product identifier or name | Optional | Match SKU or name. |
| category and active status filters | Catalogue category and Product status | Optional | Existing category and active or inactive. |
| SKU, name, category, unit, retail price, wholesale price, quantity on hand, reorder level, active status | Product results per Product Record | Yes, system set per result. Each price is shown where approved and allowed by the permissions matrix. | Apply FR-41 and matrix. |

**Actions** Search, filter and clear criteria. Owner and Manager can open Product Record or create a Product.

### Product Record

Maintain the catalogue and control approved prices.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| SKU | Product identifier | Yes | Unique and nonblank on creation. Read only thereafter. |
| name, category, unit | Product name, grouping and unit of measure | Yes, each | Nonblank text. |
| retail price, wholesale price | Approved selling prices in G$, shown separately | Required for the corresponding Customer type to buy | Read only, populated by approval. |
| Proposed retail price, proposed wholesale price | Initial or replacement value for the named price | At least one when proposing | Positive G$ amounts. Apply rule 2 before use. |
| quantity on hand | Current recorded stock | Yes, system set | Nonnegative count, initialized from reconciled opening stock or zero for a new Product without opening stock. |
| reorder level | Threshold for Low stock list inclusion | Yes | Nonnegative count. |
| active status | Whether the Product is active | Yes | Active or inactive. |

**Actions** Create or edit catalogue details. Propose prices. Review and approve prices if eligible under SEC-10.

### Stock Receiving

Record receipts per Product from a supplier delivery.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| Supplier Delivery ID | Receipt record identifier | Yes, system set | Unique. |
| product SKU | Product in the delivery | Yes | Existing active Product for posting. |
| receipt date | Date the delivery was received | Yes | Valid date. |
| Expected quantity, counted quantity | Delivery note quantity and physical count | Yes when recording a mismatch | Nonnegative counts. Must differ for a mismatch. |
| Note | Explanation of a mismatch | Yes for a mismatch | Nonblank. Apply SEC-17. |
| quantity received | Confirmed count to add | Yes for posting | Positive count confirmed by the Inventory Clerk. |
| Receipt state | Held for confirmation or posted | Yes, system set | Reflects rule 7. |

**Actions** Select Product and compare the delivery note and count. Correct input before posting. Record a mismatch, reopen a held receipt, confirm the count and post eligible stock. Review recorded receipts.

### Stock Adjustment

Record stock differences and review any required approval.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| Stock Adjustment ID | Adjustment identifier | Yes, system set | Unique. |
| product SKU | Product being adjusted | Yes | Existing Product. |
| quantity on hand | Current stock for the Product | Yes, system set | From Product. |
| quantity change | Units added or removed | Yes | Nonzero signed count. Resulting quantity on hand must be nonnegative at posting. |
| reason code | Business reason for the adjustment | Yes | One of damage, loss, count correction, return to supplier, customer return. |
| recorded by User ID, adjustment date and time | Responsible User and time of recording | Yes, system set | From the authenticated recorder and system clock. |
| Adjustment value | G$ amount for the approval check | Required before posting | Calculated under rule 8. |
| Approval state | Whether approval is required, pending or recorded | Yes, system set | Derived from rule 8 and SEC-10. |

**Actions** Inventory Clerk records adjustments and reviews adjustment records. Owner and Manager reviews and approves pending higher value adjustments and may record an adjustment in the single-authorized-user case in rule 9.

### Customer List

Find the Customer to open or select for a sale.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| Customer ID or name search | Customer identifier or name | Optional | Match Customer ID or name. |
| type filter | Customer pricing type | Optional | Retail or wholesale. |
| Customer ID, name, type, balance owed | Customer results per Customer Record | Yes, system set per result | Apply FR-42. |

**Actions** Search, filter and clear criteria. Open or create a Customer Record. Select Customer for a sale.

### Customer Record

Maintain Customer details and review related payments.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| Customer ID | Customer identifier | Yes, system set | Unique. |
| name | Customer name for invoicing and lookup | Yes | Nonblank. |
| type | Customer pricing type | Yes | Retail or wholesale. |
| contact details | Details needed for invoicing and contact | When needed | Nonblank when required for invoicing or contact. Apply SEC-16 and SEC-17. |
| balance owed | Sum of open Invoice balances in G$ | Yes, system set | Calculated by rule 5. |
| Invoice number, amount, payment date and time | Each entry in this Customer's payment history | Yes, system set per entry | Invoices linked through this Customer's sales only. |

**Actions** Create or edit Customer details. Review balance owed and payment history. Open an Invoice from the history.

### Sales List

Find sales and reach their issued Invoices.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| Sale ID or Customer name search | Sale identifier or linked Customer name | Optional | Match Sale ID or Customer name. |
| Sale date range | Date filter on sale date and time | Optional | Valid supplied dates in chronological order. |
| Sale ID, Customer ID, name, created by User ID, sale date and time, Invoice number | Sale results, creator and linked Invoice | Yes, system set per completed sale | Apply FR-43. |

**Actions** Search, filter and clear criteria. Open an issued Invoice. Start Create Sale.

### Create Sale

Enter sale lines, validate stock and complete the sale.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| Sale ID, created by User ID, sale date and time | Sale identifier, authenticated creator and transaction time | Yes, system set on the recorded Sale | Unique Sale ID. The system records the creator and time. |
| Customer ID, name, type, balance owed | Selected Customer and existing amount owed | Customer ID required. Other fields are system set. | Existing Customer. Balance owed is informational under rule 2. |
| Sale Item ID, Sale ID | Line identifier and parent Sale | Yes, system set on each recorded Sale Item | Unique Sale Item ID and an existing parent Sale. |
| product SKU, quantity on hand | Product selected for the line and current stock | Yes per line. Quantity on hand is system set. | Existing Product. Stock checked across all lines for that SKU. |
| quantity | Units requested | Yes per line | Positive count. At least one valid line is required to complete. |
| unit price | Applicable approved G$ price per unit | Yes, system set per line | Matches the price required by rule 2 at completion. |
| Line amount | Quantity multiplied by unit price in G$ | Yes, system set per line | Calculated from line inputs. |

**Actions** Select Customer. Add, correct or remove lines. Complete sale and open its Invoice. Abandon an incomplete sale.

### Invoice List

Find and open issued Invoices.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| Invoice number or Customer name search | Invoice identifier or linked Customer name | Optional | Match Invoice number or Customer name. |
| payment status filter | Invoice settlement state | Optional | Unpaid, partly paid or paid. |
| Invoice number, issue date, Customer name, calculated total, payment status, amount owed | Issued Invoice results | Yes, system set per result | Apply FR-44. |

**Actions** Search, filter and clear criteria. Open an Invoice.

### Invoice View and Print

Show the itemized Invoice, its current settlement and payments. The printable view shows the Invoice and current settlement values.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| Invoice number, issue date | Issued Invoice identifier and date | Yes, system set | Assigned at generation under rule 1. |
| Sale ID, Customer ID, name, contact details | Linked Sale and Customer details needed for invoicing | Identifiers and name required. Contact details shown when needed. | Resolve through the Invoice's Sale. |
| product SKU, name, quantity, unit price, line amount | Itemized sale lines and G$ charges | Yes, system set per line | Use recorded Sale Item quantity and unit price. |
| calculated total, amount owed, payment status | G$ total, outstanding balance and settlement state | Yes, system set | Calculated under rules 4 and 5. |
| amount | G$ payment to record | Yes for payment | Greater than zero and at or below the current amount owed. Linked to the current Invoice number. |
| Payment ID, invoice number, recorded by User ID, amount, payment date and time | Saved Payment details shown on screen | Yes, system set per Payment | Linked to this Invoice. The system records the authenticated recorder and time. |

**Actions** Print the issued Invoice, record a payment while a balance remains and review recorded payments.

### Receivables List

Show open Invoice balances in collection order.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| Invoice number, issue date, Customer name | Outstanding Invoice and associated Customer | Yes, system set per result | Issued Invoice with a positive balance. |
| Invoice age | Elapsed time since issue date | Yes, system set | Based on issue date. |
| payment status, amount owed | Unpaid or partly paid and current G$ balance | Yes, system set per result | Calculated by rule 5. |

**Actions** View the list in rule 6 order.

### Low Stock List

Show Products at or below reorder level.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| SKU, name, quantity on hand, reorder level | Product and current stock compared with its threshold | Yes, system set per result | Apply rule 10 to current Product values. |

**Actions** View the current list.

### Reports

Show the Owner and Manager the three Basic reporting outputs.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| Report | Sales by period, sales by product or stock on hand value | Yes | One of these three choices. |
| Period start and end | Date bounds for sales by period | Yes for sales by period | Valid dates. Start must be on or before end. Both dates are included. |
| Period sales total | G$ total for the selected period | Yes, system set for sales by period | Completed sales within the period only. |
| product SKU, quantity sold, sales amount | Per-Product totals for sales by product | Yes, system set per result | Calculated from completed Sale Items only. Sales amount in G$. |
| Valuation basis | Stated setting confirmed by the Owner and Manager | Required for stock on hand value | Must be confirmed and have the inputs required by that basis. |
| Stock on hand value | Current total in G$ | Required output when the basis and inputs are available | Reconciles to current quantity on hand and the stated basis. |

**Actions** Select and run a report. Confirm the valuation basis as a report setting once the business decision is made.

### User Accounts

Manage individual staff accounts and assigned access using the owner-held System Administrator permission set.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| User ID | Permanent staff account identifier | Yes, system set | Unique and retained for attribution. |
| staff name | Staff member assigned the account | Yes | Nonblank. |
| sign-in identifier | Staff login | Yes | Nonblank and unique. |
| role | Staff permissions | Yes | Owner, Sales Clerk or Inventory Clerk. Owner is the system role for the Owner and Manager. |
| Password | Credential input on setup or reset | Yes when setting a credential | Apply SEC-03 and SEC-04. Never displayed after save. |
| Account access | Whether sign-in is enabled | Yes | Enabled or disabled. Current access takes effect on subsequent requests. |
| Inactivity timeout | System-wide staff session setting | Yes | Owner and Manager sets the inactivity period under SEC-09. |

**Actions** Create and update staff accounts and role assignments, change account access, reset credentials and set the inactivity timeout.

### Audit Trail View

Review attributable business and security events.

| Field | Content | Required | Validation |
| --- | --- | --- | --- |
| user or attempted sign-in identity | User ID or the identity supplied on a failed sign-in | Yes, system set per event | Captured under SEC-12. |
| Date and time, action, record affected | When, what and the record targeted | Yes, system set per event | Captured under SEC-12. |
| Source and outcome | System function and event result | Yes, system set per event | Captured under SEC-12. |
| Before and after values | Relevant data affected by the event | Required when data changes | For creation, before is empty. Omit credentials and card data. |
| Approval details | Proposer or recorder, approver and self-approved indicator | Required for approval events | Captured under rule 9 and SEC-12. |

**Actions** View audit entries and their details.

## Business rules

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

## Roles and permissions matrix

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

## Error and validation messages

Braced values are populated from the authorized record and the current transaction.

| Trigger | Related rule or requirement | Message |
| --- | --- | --- |
| Sale quantity exceeds available stock | Rule 3, FR-22 | Only {quantity on hand} units of {product SKU} are available. Reduce the total quantity for this product or remove its lines. |
| SKU already exists | FR-02 | That SKU already exists. Open the existing product or enter a different SKU. |
| Missing or unrecognized adjustment reason | FR-09 | Choose a reason from the list before saving the adjustment. |
| Payment exceeds amount owed | Rule 5, FR-30 | The amount owed is {amount owed in G$}. Enter a payment up to that amount. |
| Sale or receipt quantity, proposed selling price or payment amount is zero or negative | FR-06, FR-17, FR-30, SEC-15 | Enter a {field name} greater than zero. |
| Quantity change is zero | FR-10, SEC-15 | Enter the number of units to add or remove. The change must be greater or less than zero. |
| Reorder level, expected quantity or counted quantity is negative | SEC-15 | Enter zero or a positive number for {field name}. |
| Adjustment would make stock negative | FR-13 | This adjustment would bring stock below zero. Check the current quantity on hand and correct the change. |
| Receipt quantities differ | Rule 7, FR-07 | The counts differ. Record the expected quantity, counted quantity and a note, then confirm the counted quantity before posting. |
| Price is missing, unapproved or no longer current | Rule 2, FR-20 | An approved price is required for {product SKU}. Refresh the line or ask the Owner and Manager to check the price. |
| Stock adjustment valuation input is missing | Rule 8 | An approved wholesale price is needed to check this adjustment. Ask the Owner and Manager to confirm the price. |
| Sign-in fails or temporary lockout applies | SEC-05 | Sign-in failed. Check your sign-in identifier and password. If you have tried several times, wait 15 minutes before trying again. |
| Signed-in user requests a function outside their permissions | SEC-06 | Your account does not have permission for this action. Ask the Owner and Manager if you need access. |
| An independent approval is required | Rule 9, SEC-10 | Ask another authorized Owner and Manager to approve this change. |

## Audit trail requirements

For each event in the table, record user or attempted sign-in identity, date and time in UTC, action, affected record identifier when one exists, source function and outcome. Capture relevant before and after values for data changes. A creation has an empty before value. Record the attempted sign-in identifier for a failed sign-in and use the User ID when a User account is attributable. Refer to SEC-12 through SEC-14 for capture, access and protection.

| Event | Record affected | Before and after values or event details |
| --- | --- | --- |
| Sale creation | Sale ID | New Sale, selected Customer and available sale details at creation. |
| Sale completion | Sale ID and linked Invoice number | Completion state, recorded Sale Items and affected stock quantities. |
| Stock adjustment | Stock Adjustment ID and product SKU | Quantity change, reason code, state and quantity on hand before and after posting. |
| Price change | Product SKU and applicable price | Previously approved price and newly approved price. For an initial approval, the previous price is empty. |
| Payment recording | Payment ID and Invoice number | Recorded amount, Invoice amount owed and payment status before and after posting. |
| Sign-in success or failure | User ID or attempted sign-in identifier | Password sign-in and outcome, including temporary lockout when applicable. Credential values are excluded. |
| Sign-out or session expiry | User ID | Session end and reason. Session secrets are excluded. |
| Permission denial | Affected function or record identifier when known | Requested action and outcome. No protected record contents. |
| User account change | User ID | Changed account details and access values, with credential changes identified by event only. |
| Approval, including self-approval | Product SKU and price proposal, or Stock Adjustment ID | Approval state before and after, proposer or recorder User ID, approver User ID and self-approved indicator. |

## Security requirements

OWASP ASVS Level 1 is the project baseline for a staff application holding customer names, contact details, and amounts invoiced, paid and owed. Payment recording captures amounts paid against invoices. Card data and card payment processing are outside Release 1. The BRD uses OWASP Top 10 as its risk reference. The project adds the selected controls below where stock and money movement create fraud risk. [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/)

| ID | The system shall | Stories and business requirements |
| --- | --- | --- |
| SEC-01 | Assign each staff member one individual User account with a unique User ID and sign-in identifier. Shared accounts are prohibited. Start a staff session only after validating the credentials of an enabled account under SEC-05. Attribute authorized sales, stock adjustments and payments to the authenticated account. | US-20, US-21 / BR-15, BR-16 |
| SEC-02 | Allow only the Owner and Manager to manage individual staff accounts and their assigned roles, using the permissions matrix. Changes to account access take effect on subsequent requests and preserve original attribution for past records. | US-21 / BR-15, BR-16 |
| SEC-03 | Require a minimum of 15 characters whenever a staff password is set or changed. | US-20, US-21 / BR-15, BR-16 |
| SEC-04 | Store passwords only as salted hashes using a password hashing function. Passwords must never be stored as plaintext or in reversible form. | US-21 / BR-15, BR-16 |
| SEC-05 | Lock sign-in for an account for 15 minutes after 5 consecutive failed attempts. A successful sign-in or expiry of the lockout resets the failed attempt count. Show the same sign-in failure message for invalid credentials, unknown identifiers and temporarily locked accounts. | US-20, US-22 / BR-15, BR-19 |
| SEC-06 | Enforce current role permissions on the server for every protected request, including function, record and field access, direct addresses and searches. Deny unauthorized requests without exposing protected data or changing records. | US-02, US-20, US-23 / BR-15, BR-17, BR-20 |
| SEC-07 | Redirect a signed-out staff user requesting a protected screen, including a direct Invoice address, to Sign In before returning any protected details. Reject unauthenticated protected requests. | US-20 / BR-15 |
| SEC-08 | Return application content and accept credentials or other application input only over HTTPS. | US-20 / BR-15 |
| SEC-09 | End staff sessions after the inactivity period set by the Owner and Manager, initially 15 minutes. Reject requests using an expired session and require fresh sign-in. | US-20 / BR-15 |
| SEC-10 | Enforce rule 9 at approval time for initial prices, price changes and stock adjustments above the threshold. Check the approver's current authorization, whether the approver is the proposer or recorder, and whether a second authorized approver is available. The audit trail identifies the proposer or recorder and approver, including permitted self-approval. | US-02, US-05 / BR-17, BR-18, BR-19 |
| SEC-11 | End the authenticated session on sign-out so that subsequent requests using that session cannot reach protected data or actions. | US-20 / BR-15 |
| SEC-12 | Capture the events and required metadata in Audit trail requirements, including synchronized UTC timestamps, authentication method for sign-ins, and relevant before and after values. Record an associated audit entry for every completed audited business action. Exclude credential values, session secrets and card data. | US-02, US-04, US-05, US-13, US-21, US-22 / BR-16, BR-19 |
| SEC-13 | Allow only the Owner and Manager to view audit entries. | US-22 / BR-15, BR-19 |
| SEC-14 | Prevent every system user, including the Owner and Manager, from editing or deleting audit entries. | US-22 / BR-19 |
| SEC-15 | Validate form inputs and search criteria on the server. Reject negative quantities and monetary amounts and require positive values for sale quantity, quantity received, proposed selling prices and payment amount. An adjustment's signed quantity change is a nonzero difference, subject to FR-13. | US-01, US-03, US-04, US-07, US-13, US-23 / BR-01, BR-02, BR-03, BR-05, BR-09, BR-20 |
| SEC-16 | Limit collection of customer data to the name, type and contact details needed for invoicing and contact. | US-06 / BR-04 |
| SEC-17 | Prevent card data from being stored in any field. Staff must never enter card numbers or any other card data in free text. Verify this business process rule during UAT before sign off. | US-06, US-13 / BR-04, BR-09 |
| SEC-18 | Calculate Invoice totals from validated Sale Items and reject a manually supplied Invoice total. | US-11 / BR-08 |
| SEC-19 | Complete a backup of system data each day, with the outcome recorded so that the Owner and Manager can verify daily completion. | US-24 / BR-21 |
| SEC-20 | Support a restore from backup for verification before Release 1 sign off. Use fictional sample data and reconcile restored stock, Customer records, sales, invoices, payments and balances to the backup. Keep the restore criterion open after any failed restore or discrepancy until a successful restore and reconciliation are demonstrated. | US-24 / BR-21 |
| SEC-21 | Protect quantity on hand from going below zero on sale completion, including during concurrent sales or stock changes. | US-09, US-10 / BR-06, BR-07 |

### Selected Level 2 controls

- **Audit event detail and protection, SEC-12 to SEC-14.** Use structured event metadata, synchronized timestamps, authentication and denied-access event capture, and protected audit records. These controls let the Owner and Manager reconstruct disputed payments, unauthorized access and unexplained stock movements. The ASVS 5.0.0 references are 16.2.1, 16.2.2, 16.3.1, 16.3.2 and 16.4.2. [OWASP ASVS V16](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x25-V16-Security-Logging-and-Error-Handling.md)
- **Separation of duties on prices and stock adjustments, SEC-06 and SEC-10.** Document and enforce who can write approved prices and approval state, and enforce the adjustment approval threshold as a business logic limit. These selected Level 2 controls are 8.1.2, 8.2.3 and 2.3.2. Rule 9 adds the project's independent-approval rule and the audited sole-authorized-approver exception to reduce unauthorized discounts and unexplained stock loss. ASVS 5.0.0 lists its general high-value multi-user approval requirement, 2.3.5, at Level 3. [OWASP ASVS V8](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x17-V8-Authorization.md), [OWASP ASVS V2](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x11-V2-Validation-and-Business-Logic.md)

## Non functional requirements

### Performance

Routine screens and searches shall respond without undue delay under normal concurrent use at one small store. The assumed volume is one electronics and accessories catalogue and one store's Release 1 Customer, Sale, Invoice and Payment history, used concurrently by the counter, stock and management staff.

### Backup and recovery

Daily backup and pre-sign-off restore verification are specified in SEC-19 and SEC-20.

### Usability

Use consistent labels and layouts for staff lists and records, with the plain language validation messages specified above. The printable Invoice shall remain legible on standard paper, with itemized lines, totals and settlement status visible.

## Change log

| Version | Date | Change |
| --- | --- | --- |
| 1 | November 24, 2026 | Initial Release 1 functional specification. |
