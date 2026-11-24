# Stakeholder and Scope Analysis

**Author** Brandon Narine  
**Date** November 24, 2026  
**Status** Initial draft for Phase 1 business analysis. The system has yet to be built.

## Purpose

This artifact identifies who the proposed inventory and invoicing system will serve and what those stakeholders need it to support. It defines the Release 1 boundary and priorities for a fictional electronics and accessories retailer and records the decisions that need business confirmation before scope sign off.

## Stakeholder register

| Stakeholder | Interest in the system | Influence | Main concerns | Engagement or communication |
| --- | --- | --- | --- | --- |
| Store Owner and Manager | Controls pricing and user access, approves higher value stock adjustments, and monitors sales, low stock, and receivables. | High | Price accuracy, stock loss, overdue balances, and accountability for approvals. | Review scope and policy defaults with the owner, walk through reports and approval scenarios, and obtain scope sign off. |
| Sales Clerk | Checks stock, records sales and payments, and issues invoices at the counter. | Medium | Fast product lookup, correct retail or wholesale prices, clear stock warnings, and accurate payment status. | Walk through a counter sale and a partial payment, then ask the clerk to review the invoice and counter workflow. |
| Inventory Clerk | Records supplier deliveries and stock adjustments and investigates count differences. | Medium | Accurate stock counts, clear adjustment reasons, and a record of who changed stock. | Walk through a supplier delivery and a count correction with the clerk and validate the reason codes. |
| Customer | Receives invoices and payment receipts for retail or wholesale purchases. | Low | Clear prices, correct amounts paid and owed, and a usable copy of the invoice. | Ask retail and wholesale customer representatives to review fictional invoice and payment examples through the sales counter. |
| System Administrator role | The owner holds this role and manages user accounts and permissions. | High | Appropriate access for each clerk, individual accounts, and recoverable records. | Review the proposed permissions with the owner and cover this role as a permission set in the Functional Specification. |

## Personas

All four persona names are fictional. The owner also holds the System Administrator role, which is covered as a permission set.

### Owner and Manager: Alina Veylan

- **Goals** See what is selling, what is running low, and who still owes money. Keep prices and stock changes accountable.
- **Current pain points** Stock counts drift across paper and spreadsheets. Manual invoices vary, and unpaid balances take time to reconstruct.
- **Top tasks** Approve product prices and price changes, manage user accounts, review sales, low stock, and receivables reports, and approve stock adjustments above the business threshold.

### Sales Clerk: Devren Marlowe

- **Goals** Complete counter sales promptly and give customers accurate invoices and payment information.
- **Current pain points** Manual stock checks slow sales, price lookups can use stale records, and partial payments are difficult to reconcile.
- **Top tasks** Search products, check availability, create sales at the applicable price, generate and print invoices, and record full and partial payments.

### Inventory Clerk: Maris Fenwick

- **Goals** Keep recorded stock aligned with the shelf count and explain discrepancies.
- **Current pain points** Paper delivery notes and spreadsheets require manual updates, and the current process leaves no reliable audit trail for adjustments.
- **Top tasks** Receive stock against supplier deliveries, record stock adjustments with reasons, and investigate differences between physical and recorded stock.

### Customer: Talia Norwyn

- **Goals** Receive clear charges and proof of payment for retail or wholesale purchases.
- **Current pain points** Manually prepared invoices can be inconsistent, and the amount remaining after a partial payment can be hard to confirm.
- **Top tasks** Receive an itemized invoice and payment receipts from staff and confirm what has been paid and what is still owed. Customers have no Release 1 login.

## Scope statement

### In scope for Release 1

| Feature | Release 1 scope |
| --- | --- |
| Product catalogue | SKU, name, category, unit, retail price, wholesale price, quantity on hand, reorder level, and active status. |
| Stock receiving | Receive stock against a supplier delivery and record quantity and date. |
| Stock adjustments | Require a reason code of damage, loss, count correction, return to supplier, or customer return. |
| Customer records | Store name, type (retail or wholesale), contact details, and balance owed. |
| Sale creation | Select a customer and add product lines. Validate quantity and price, and block a sale line when quantity on hand is insufficient. |
| Invoice generation | Generate a unique sequential invoice number, sale lines, totals, payment status, and a printable invoice view. |
| Payments | Record full and partial payments against an invoice and show payment history per customer. |
| Receivables list | Show unpaid and partly paid invoices sorted by age. |
| Low stock list | Show products at or below their reorder level. |
| Basic reporting | Show sales by period, sales by product, and stock on hand value. |
| User roles and permissions | Apply permissions for the Owner, Sales Clerk, and Inventory Clerk. |
| Search and filtering | Support search and filtering on products, customers, sales, and invoices. |

### Out of scope for Release 1

Sections 5.2 and 5.3 provide the exclusions and the named roadmap commitments. Proposed assignments below are for business review. Release 4 is a proposed planning placeholder.

| Excluded feature | Future release |
| --- | --- |
| Online store or customer self service portal | Release 3 for customer portal evaluation. Release 4 proposed for portal implementation and an online store, subject to that evaluation and approval. |
| Card payment processing and payment gateways | Release 4 proposed. |
| Accounting, payroll, and tax filing | Release 4 proposed. |
| Barcode scanner and label printer hardware integration | Release 2 includes barcode support. Release 2 proposed for scanner and label printer hardware integration, subject to confirmation of the devices covered. |
| Multiple store locations and stock transfers between locations | Release 3 includes second location support. Release 3 proposed for stock transfers between locations. |
| Delivery scheduling and routing | Release 4 proposed. |
| Supplier purchase orders and automatic reordering | Release 2 includes supplier purchase orders. Release 4 proposed for automatic reordering. |
| Demand forecasting | Release 3 includes a forecasting report. Release 3 proposed for demand forecasting within that report. |

Release 1 covers staff operations at one store, from recording stock and validating availability through completing sales, deducting stock, issuing printable invoices, and tracking payments and outstanding balances. Staff serve retail and wholesale customers directly. Payment recording captures amounts paid against invoices. Customers receive documents through staff and have no system account.

## MoSCoW prioritization

Section 5.3 includes all 12 Section 5.1 features in the Release 1 baseline. Must, Should, and Could indicate relative build priority within that baseline. Any change to the baseline requires business approval.

### Must have

- **Product catalogue.** Provides the product, price, stock, and reorder data needed for sales and inventory.
- **Stock receiving.** Records increases in stock against supplier deliveries.
- **Stock adjustments.** Keeps stock accurate and records the reason for each correction or return.
- **Customer records.** Establishes the customer type and the balance owed.
- **Sale creation.** Validates quantities, prices, and stock availability before completion.
- **Invoice generation.** Gives each sale a consistent invoice with a unique number and printable view.
- **Payments.** Records full and partial payments and supports accurate balances.
- **Receivables list.** Gives the owner a view of unpaid and partly paid invoices by age.
- **Low stock list.** Gives staff visibility when products reach their reorder level.
- **User roles and permissions.** Restricts pricing, stock, and sales actions to authorized staff.

### Should have

- **Search and filtering.** Helps staff find products, customers, sales, and invoices during routine work.

### Could have

- **Basic reporting.** Adds summary views of sales and stock value once the underlying records are available.

### Will not have this release

- Online store or customer self service portal.
- Card payment processing and payment gateways.
- Accounting, payroll, and tax filing.
- Barcode scanner and label printer hardware integration.
- Multiple store locations and stock transfers between locations.
- Delivery scheduling and routing.
- Supplier purchase orders and automatic reordering.
- Demand forecasting.

## Assumptions, constraints, and dependencies

### Assumptions

- The first release serves a single electronics and accessories store with retail and wholesale customers.
- The owner also handles user administration. Stakeholder engagement and business policy defaults are proposed for review.
- The current process uses paper and spreadsheets. Any examples used during analysis are fictional, with invented amounts expressed in G$.

### Constraints

- Release 1 is a web based system for internal staff. Its role model covers the Owner, Sales Clerk, and Inventory Clerk.
- Payment recording uses invoice and payment amounts. Card data collection, storage, and processing are outside scope.
- Planned security requirements follow OWASP ASVS Level 1 because the system will hold customer names, contact details, and records of amounts invoiced, paid, and owed. Selected Level 2 controls apply to audit logging and separation of duties for price changes and stock adjustments because these actions affect money or stock. OWASP Top 10 is the risk reference for the Business Requirements Document.

### Dependencies

- Reliable starting stock depends on reconciling spreadsheet and paper records against physical counts. Opening customer balances depend on reconciliation against unpaid invoices and payment records.
- Sales and low stock reporting depend on the owner confirming catalogue prices and reorder levels. Access and approval workflows depend on the owner confirming staff permissions and the adjustment approval threshold.
- Final invoice and payment handling depends on business confirmation of the policy and document questions below.

## Open questions before scope sign off

The proposed defaults will be used for analysis if the business provides no answer. They remain subject to scope sign off.

| Question | Proposed default |
| --- | --- |
| Is the wholesale price fixed per product or negotiated for each customer? | Use the owner approved wholesale price on the product record for every wholesale customer. Treat customer specific prices as a scope change. |
| How long must invoices and payment records be kept? | Retain invoices and payment records throughout Release 1. Defer any automatic deletion until the owner confirms a retention period and the applicable obligations. |
| Can staff complete a sale for a customer with an unpaid balance? | Allow the sale and make the existing balance visible to the Sales Clerk during customer selection. |
| Who can approve a stock adjustment above a set value, and what is that value? | The Store Owner and Manager approves adjustments above the threshold. Use G$10,000 as an invented threshold for analysis. Use a separate authorized approver whenever a second user is available. |
| Must tax appear on the invoice, and what description should appear if it does? | Omit a separate tax line from the fictional draft invoice pending confirmation. If the owner confirms that tax must be shown, use the business approved tax name, rate, and amount as separate invoice details. |
| Do customers need printed invoices, emailed invoices, or both? | Provide printed invoices using the Release 1 printable view. Treat automated emailing as a scope change. |
| Do customers require a separate payment receipt for each payment? | Use the updated printed invoice and the recorded payment history to confirm payments with customers. Confirm whether a separate receipt document is needed, since Section 5.1 defines payment history and printable invoices. |

## Change log

| Version | Date | Change |
| --- | --- | --- |
| 1 | November 24, 2026 | Initial draft. |
