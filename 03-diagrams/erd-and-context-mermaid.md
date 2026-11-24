# System Context Diagram and Entity Relationship Diagram (Mermaid)

**Author** Brandon Narine
**Date** November 24, 2026
**Status** Phase 1 business analysis. The system has yet to be built.

This file holds the Mermaid versions of the system context diagram and the entity relationship diagram from `diagram-outlines.md`, so both render directly on GitHub. The content follows the outlines exactly. Polished draw.io versions remain a Phase 2 design activity, as recorded in the implementation roadmap.

## 1. System Context Diagram

The Inventory and Invoicing System sits at the centre. The three staff roles use it, the Customer receives printed invoices and payment confirmations without a system login, and the system replaces the paper and spreadsheet records used today.

```mermaid
flowchart LR
    OM["Owner and Manager"]
    SC["Sales Clerk"]
    IC["Inventory Clerk"]
    SYS["Inventory and Invoicing System"]
    CUST["Customer"]
    PAPER["Paper and Spreadsheet Records"]

    OM -->|"Approve prices and higher value stock adjustments. Manage user accounts."| SYS
    SYS -->|"Receivables list, Low stock list, Basic reporting"| OM
    SC -->|"Look up products and prices, check stock, maintain customer records, create sales, record payments"| SYS
    SYS -->|"Printable invoices, payment status, customer balances, payment history"| SC
    IC -->|"Record stock received and stock adjustments with reasons"| SYS
    SYS -->|"Stock on hand and adjustment records for count checks"| IC
    SC -->|"Issue printed invoices and confirm payments"| CUST
    SYS -->|"Replaces manual stock, invoice, and payment records"| PAPER
```

## 2. Entity Relationship Diagram

Nine entities. Identifiers and foreign keys are marked on the attributes. Cardinalities in words, matching the outline: a Customer places zero or many Sales and each Sale belongs to one Customer; a completed Sale has one or more Sale Items; each completed Sale produces one Invoice; an Invoice receives zero or many Payments; a Product can have zero or many Stock Adjustments and Supplier Delivery records; a User can create Sales and record Payments and Stock Adjustments, and each of those records identifies one responsible User.

```mermaid
erDiagram
    CUSTOMER ||--o{ SALE : "places"
    SALE ||--|{ SALE_ITEM : "contains"
    PRODUCT ||--o{ SALE_ITEM : "appears on"
    SALE ||--|| INVOICE : "produces"
    INVOICE ||--o{ PAYMENT : "receives"
    PRODUCT ||--o{ STOCK_ADJUSTMENT : "has"
    PRODUCT ||--o{ SUPPLIER_DELIVERY : "received on"
    USER ||--o{ SALE : "creates"
    USER ||--o{ PAYMENT : "records"
    USER ||--o{ STOCK_ADJUSTMENT : "records"

    PRODUCT {
        string SKU PK
        string name
        string category
        string unit
        decimal retail_price
        decimal wholesale_price
        int quantity_on_hand
        int reorder_level
        boolean active_status
    }
    CUSTOMER {
        int customer_id PK
        string name
        string type
        string contact_details
        decimal balance_owed
    }
    SALE {
        int sale_id PK
        int customer_id FK
        int created_by_user_id FK
        datetime sale_date_and_time
    }
    SALE_ITEM {
        int sale_item_id PK
        int sale_id FK
        string product_SKU FK
        int quantity
        decimal unit_price
    }
    INVOICE {
        int invoice_number PK
        int sale_id FK
        date issue_date
        decimal calculated_total
        string payment_status
    }
    PAYMENT {
        int payment_id PK
        int invoice_number FK
        int recorded_by_user_id FK
        decimal amount
        datetime payment_date_and_time
    }
    STOCK_ADJUSTMENT {
        int stock_adjustment_id PK
        string product_SKU FK
        int recorded_by_user_id FK
        int quantity_change
        string reason_code
        datetime adjustment_date_and_time
    }
    SUPPLIER_DELIVERY {
        int supplier_delivery_id PK
        string product_SKU FK
        int quantity_received
        date receipt_date
    }
    USER {
        int user_id PK
        string staff_name
        string sign_in_identifier
        string role
    }
```

## Change log

| Version | Date | Change |
| --- | --- | --- |
| 1 | November 24, 2026 | Mermaid versions of the system context diagram and the entity relationship diagram, built from the approved diagram outlines. |
