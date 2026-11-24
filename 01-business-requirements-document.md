# Business Requirements Document

**Project** Inventory and Invoicing System  
**Author** Brandon Narine  
**Date** November 24, 2026  
**Status** Initial draft for Phase 1 business analysis. The system has yet to be built.

## Introduction and purpose

This document defines what a fictional single-store electronics and accessories retailer needs from its planned Inventory and Invoicing System. It sets out the business outcomes, Release 1 scope, and criteria the business can use to judge a future build. The business requirements are the starting point for the functional specification and acceptance planning. Stakeholders, personas, scope, and priorities follow the approved [Stakeholder and Scope Analysis](02-stakeholder-and-scope-analysis.md).

## Business problem

The store tracks stock in spreadsheets and on paper and prepares invoices manually. Staff can spend time checking records and still quote quantities that differ from what is on the shelf. Invoices take time to prepare and can differ in how they present charges and balances. The store has no reliable warning when stock runs low, no single record of what customers owe, and no audit trail that explains who adjusted stock and why. The owner has to reconstruct records to see what is selling, what is running low, and who still owes money.

## Business objectives

Collect the initial measurements during a pre-build baseline period using the current workflow. The figures are unknown. The owner will confirm the measurement periods and the invoice time improvement target after reviewing the baseline. Measure the same outcomes after Release 1 goes live.

| Objective | Measure and success threshold |
| --- | --- |
| Reduce time to produce an invoice. | Time representative counter sales from the start of invoice preparation until an itemized invoice is ready to issue. Compare the median for comparable sales before and after rollout. Success means a lower median that meets the improvement target agreed after baseline measurement. |
| Eliminate sales recorded against stock that is unavailable. | Count completed sale lines where the quantity sold exceeded the stock available when the sale was recorded. Use contemporaneous stock checks during the baseline and reconcile sales with stock movements and physical count checks after rollout. Success means zero such lines during the agreed review period. |
| Give the owner a same-day view of unpaid balances. | At the close of each business day in the review period, compare all unpaid and partly paid balances in the owner's view with invoices and payments recorded through that day's close. Success means the balances are available and reconcile on the same day. During the baseline, record whether the owner can obtain the equivalent view by close and how long reconciliation takes. |

## Project objectives and success criteria

These criteria describe what the business will verify in the future Release 1 implementation.

| Project objective | Release 1 success criteria |
| --- | --- |
| Establish one reliable inventory record. | Staff can maintain the catalogue, record a supplier delivery, explain a stock adjustment, and see products at or below reorder level. Opening stock reconciles to physical counts. |
| Support the counter sale from stock check to invoice. | Staff can create retail and wholesale sales at the applicable approved price. A sale for the last available unit succeeds and deducts that unit. An attempt to sell more than the available stock is blocked. Completed sales yield printable, itemized invoices with unique sequential numbers and payment status. |
| Make payments and balances reconcilable. | Staff can record partial and full payments against invoices, find payment history by customer, and verify remaining balances. The owner's age-sorted list includes unpaid and partly paid invoices and drops an invoice once it is fully paid. |
| Make daily records and management summaries usable. | Staff can search and filter products, customers, sales, and invoices. The owner can review sales by period and by product and reconcile those reports to recorded sales. The owner can review stock on hand value using the approved valuation basis. |
| Make staff actions accountable and records recoverable. | Role checks confirm the intended access for the Owner, Sales Clerk, and Inventory Clerk. Price and stock approval scenarios and audit entries show who acted and who approved. A backup of sample business records is restored successfully before sign off. |

## Current state summary

The Sales Clerk looks up products and prices, checks availability manually, prepares a handwritten or spreadsheet invoice, and records the sale and payments. Stock is updated separately. A partial payment may require the Sales Clerk to compare the invoice with other payment records to establish what remains due.

The Inventory Clerk uses paper supplier delivery notes and spreadsheet records to update stock and investigates discrepancies by comparing records with the physical count. The Store Owner and Manager relies on those same records to check stock, sales, and unpaid balances. Customers receive their invoices and payment information through staff.

## Proposed solution summary

A single web based system will hold products, stock, customers, sales, invoices, and payments. It will validate stock before a sale is completed, generate a consistent printable invoice, deduct stock automatically, and report low stock and unpaid balances.

## Stakeholders and personas

The approved baseline defines these fictional personas and their responsibilities.

| Stakeholder | Approved persona | Release 1 responsibilities or needs |
| --- | --- | --- |
| Store Owner and Manager | Owner and Manager, Alina Veylan | Approves product prices and price changes, manages accounts, views sales, low stock, and receivables, and approves higher value stock adjustments. |
| Sales Clerk | Sales Clerk, Devren Marlowe | Checks stock, records sales and payments, and generates and prints invoices. |
| Inventory Clerk | Inventory Clerk, Maris Fenwick | Records supplier deliveries and stock adjustments and investigates count differences. |
| Customer | Customer, Talia Norwyn | Receives invoices and confirmation of payments through staff for retail or wholesale purchases. |
| System Administrator role | Held by the Store Owner and Manager | Manages accounts and permissions under the owner's permission set. |

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

## Assumptions, constraints, and dependencies

### Assumptions

- Release 1 serves one electronics and accessories store with retail and wholesale customers. The Store Owner and Manager also handles user administration.
- The current process uses paper and spreadsheets. Stakeholder engagement and policy defaults remain subject to business review.
- The personas are fictional. Any examples used in later analysis will use invented data and express sample amounts in Guyana dollars (G$).

### Constraints

- Release 1 is a web based system for internal staff. Its role model covers the Owner, Sales Clerk, and Inventory Clerk. Customers receive documents through staff and have no Release 1 login.
- Payment recording covers invoice and payment amounts. Card data collection, storage, and processing are outside Release 1.
- Section 8 of the controlling spec sets the security requirements framework and the business risks summarized below.

### Dependencies

- Reconcile paper and spreadsheet stock records to physical counts and reconcile opening customer balances to unpaid invoices and payment records.
- Confirm catalogue prices and reorder levels for sales and low stock reporting. Confirm staff permissions and the stock adjustment approval threshold for access and approval workflows.
- Confirm the invoice and payment policies recorded in Artifact 2 before finalizing those workflows.

## Business risks

| Risk | Mitigation |
| --- | --- |
| Inaccurate opening stock could cause staff to refuse valid sales or promise stock the store lacks. | Reconcile opening quantities to a physical count and resolve discrepancies before rollout. |
| Incorrect opening balances could cause collection errors and payment disputes. | Reconcile amounts owed to unpaid invoices and recorded payments before entering opening balances. |
| Staff may revert to paper during busy periods, leaving sales, stock, and balances out of step. | Train staff on the counter workflow, rehearse busy-period sales during acceptance, and reconcile records during the planned parallel run. |
| Incorrect prices or unexplained stock changes could cause financial loss. | Review approved prices before rollout and check approvals and audit history when investigating discrepancies. |
| Loss of records could interrupt sales and leave the store unable to prove what is owed. | Back up business records daily and verify a restore with fictional sample data before sign off. |

## Security risk summary

This project follows OWASP ASVS (Application Security Verification Standard) Level 1 as the baseline for a web application that holds customer names, contact details, and records of amounts invoiced, paid, and owed. Selected Level 2 controls apply to audit logging and separation of duties on price changes and stock adjustments because those actions can affect money and stock. PCI DSS does not apply because card data and card payment processing are outside scope. The Functional Specification will define the detailed controls.

The risk reference is [OWASP Top 10 (2021)](https://owasp.org/Top10/2021), which matches the category names in Section 8.2 of the controlling spec.

| Category | Business consequence |
| --- | --- |
| Broken Access Control | A clerk who reaches owner-only price or approval functions could change prices without authority or conceal stock loss. |
| Injection | Unvalidated input in searches or forms could expose or change customer, sale, or stock records, disrupting sales and reconciliation. |
| Identification and Authentication Failures | Shared or weak sign-ins could allow unauthorized actions and make stock and payment changes impossible to attribute reliably. |
| Security Logging and Monitoring Failures | Missing or unreliable logs could prevent the owner from investigating stock loss, unauthorized price changes, or disputed payments. |

### Reviewed categories with lower direct relevance to the Release 1 business scope

| Category | Reason for lower direct relevance |
| --- | --- |
| Cryptographic Failures | The store handles no card data in Release 1. Staff credentials, customer contact details, and balances still need protection. |
| Insecure Design | The scope is limited to one store and staff workflows. Planned stock validation, role boundaries, approvals, and traceability address the main design risks identified for money and stock. |
| Security Misconfiguration | Release 1 has a staff-only access model without a customer portal or payment gateway. Secure deployment and configuration still matter. |
| Vulnerable and Outdated Components | This is primarily an implementation and maintenance concern. No specific software components have been selected in the business scope. |
| Software and Data Integrity Failures | The Release 1 scope specifies no external feeds for ongoing stock or payment updates. Software updates, opening data, and backup integrity still matter. |
| Server-Side Request Forgery (SSRF) | The business workflows do not call for retrieving content from staff-supplied web addresses or from external integrations. |

## Glossary

| Term | Meaning |
| --- | --- |
| SKU | Stock keeping unit, the store's unique identifier for a product it tracks. |
| Stock on hand | The quantity the store currently holds, as reflected in the inventory record and checked against physical counts. |
| Reorder level | The stock quantity at or below which a product appears on the low stock list. |
| Stock receiving | Recording stock added to inventory against a supplier delivery. |
| Stock adjustment | A change to recorded stock to account for damage, loss, count correction, return to supplier, or customer return. |
| Retail | The customer type served at the approved retail price. |
| Wholesale | The customer type eligible for approved wholesale pricing. The precise pricing policy is subject to sign off. |
| Invoice | The numbered, itemized record of a sale showing charges, totals, and payment status. |
| Partial payment | A payment of less than the outstanding invoice balance, leaving an amount still owed. |
| Receivables | Amounts customers still owe on unpaid or partly paid invoices. |
| Invoice age | Time elapsed since the invoice was issued, used to order outstanding invoices. |
| Audit trail | The record of who performed or attempted a tracked action, what happened, and when. |

## Open questions carried forward

The seven sign-off questions and their proposed defaults are recorded in [Artifact 2, Open questions before scope sign off](02-stakeholder-and-scope-analysis.md#open-questions-before-scope-sign-off). Those proposed defaults remain subject to business confirmation.

**New BRD question.** Which valuation basis should the owner use for the stock on hand value report required by BR-14? The owner will confirm the basis before the report is specified.

## Change log

| Version | Date | Change |
| --- | --- | --- |
| 1 | November 24, 2026 | Initial draft of the Business Requirements Document. |
