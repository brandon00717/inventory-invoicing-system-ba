# Inventory and Invoicing System Diagram Outlines

**Author** Brandon Narine  
**Date** November 24, 2026  
**Status** Draft for Phase 1 business analysis. The system has yet to be built.

These outlines are the content source for the draw.io diagrams in Section 7.2 of the project spec. The draw.io versions will be built and polished after the functional scope is stable.

## 1. System Context Diagram

Show who uses the proposed system, what the Customer receives, and which current method the system replaces.

- Centre the Inventory and Invoicing System.
- Show the three internal roles: Owner and Manager, Sales Clerk, and Inventory Clerk.
- Show the Customer as an external recipient with no system login.
- Show Paper and Spreadsheet Records as the replaced current method.

| Source | Destination | Line label |
| --- | --- | --- |
| Owner and Manager | Inventory and Invoicing System | Approve product prices and price changes and higher value stock adjustments. Manage user accounts. |
| Inventory and Invoicing System | Owner and Manager | Provide the Receivables list, Low stock list, and Basic reporting. |
| Sales Clerk | Inventory and Invoicing System | Look up products and prices, check stock, maintain customer records, create sales, and record payments. |
| Inventory and Invoicing System | Sales Clerk | Provide printable invoices, invoice payment status, customer balances, and payment history. |
| Inventory Clerk | Inventory and Invoicing System | Record stock received against supplier deliveries and stock adjustments with reasons. |
| Inventory and Invoicing System | Inventory Clerk | Provide stock on hand and adjustment records for count checks. |
| Sales Clerk | Customer | Issue printed invoices and confirm payments. |
| Inventory and Invoicing System | Paper and Spreadsheet Records | Replaces manual stock, invoice, and payment records. |

## 2. Use Case Diagram

Place use cases inside the Inventory and Invoicing System boundary. Place Owner and Manager, Sales Clerk, and Inventory Clerk outside it, with solid association lines to their use cases.

| Use case | Primary actor | Release 1 feature |
| --- | --- | --- |
| Maintain Product Catalogue | Owner and Manager | Product catalogue |
| Approve Product Prices and Price Changes | Owner and Manager | Product catalogue |
| Receive Stock | Inventory Clerk | Stock receiving |
| Adjust Stock | Inventory Clerk | Stock adjustments |
| Approve Higher Value Stock Adjustments | Owner and Manager | Stock adjustments |
| Manage Customer Records | Sales Clerk | Customer records |
| Create Sale | Sales Clerk | Sale creation |
| Validate Stock Availability | Sales Clerk, through Create Sale | Sale creation |
| Generate Invoice | Sales Clerk, through Create Sale | Invoice generation |
| Record Payment | Sales Clerk | Payments |
| View Receivables | Owner and Manager | Receivables list |
| View Low Stock | Owner and Manager | Low stock list |
| Run Basic Reports | Owner and Manager | Basic reporting |
| Manage User Accounts | Owner and Manager | User roles and permissions |
| Search and Filter Records | Sales Clerk | Search and filtering |

Draw dashed dependency arrows from the including use case to the included use case. Label each arrow `«include»`.

- **Create Sale → Validate Stock Availability.** Every sale must pass the availability check before it can complete (BR-06).
- **Create Sale → Generate Invoice.** Every completed sale produces a printable, itemized invoice (BR-08).

## 3. Entity Relationship Diagram

### Key attributes

| Entity | Key attributes |
| --- | --- |
| Product | SKU (identifier), name, category, unit, retail price, wholesale price, quantity on hand, reorder level, active status |
| Customer | Customer ID (identifier), name, type, contact details, balance owed |
| Sale | Sale ID (identifier), Customer ID (foreign key), created by User ID (foreign key), sale date and time |
| Sale Item | Sale Item ID (identifier), Sale ID (foreign key), product SKU (foreign key), quantity, unit price |
| Invoice | Invoice number (sequential identifier), Sale ID (foreign key), issue date, calculated total, payment status |
| Payment | Payment ID (identifier), invoice number (foreign key), recorded by User ID (foreign key), amount, payment date and time |
| Stock Adjustment | Stock Adjustment ID (identifier), product SKU (foreign key), recorded by User ID (foreign key), quantity change, reason code, adjustment date and time |
| Supplier Delivery | Supplier Delivery ID (identifier), product SKU (foreign key), quantity received, receipt date |
| User | User ID (identifier), staff name, sign-in identifier, role |

### Relationships

- A Customer can place zero or many Sales. Each Sale belongs to one Customer.
- A completed Sale has one or more Sale Items. Each Sale Item belongs to one Sale.
- A Product can appear on zero or many Sale Items. Each Sale Item refers to one Product.
- Each completed Sale produces one Invoice. Each Invoice belongs to one Sale. The invoice presents its sale lines through that Sale.
- An Invoice can receive zero or many Payments. Each Payment applies to one Invoice. Payment history can be traced to the Customer through the Invoice and Sale.
- A Product can have zero or many Stock Adjustments. Each Stock Adjustment refers to one Product.
- A Product can appear on zero or many Supplier Delivery records. Each record represents the quantity received for one Product. Grouping multiple products from the same supplier delivery is open.
- A User can create zero or many Sales, record zero or many Payments, and record zero or many Stock Adjustments. Each of those records identifies one responsible User.
