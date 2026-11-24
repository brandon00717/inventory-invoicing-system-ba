# Test Cases

**Project** Inventory and Invoicing System  
**Author** Brandon Narine  
**Date** November 24, 2026  
**Status** Phase 1 business analysis. The system has yet to be built. These cases are written for execution against the built system in a later phase.

These cases cover Release 1 functional and security requirements. The security baseline is OWASP ASVS Level 1, with selected Level 2 controls for audit logging and separation of duties. The BRD uses OWASP Top 10 as its risk reference. Functional cases follow the areas in the [Functional Specification](05-functional-specification.md), followed by security cases. All test data is fictional sample data created for testing.

## Test data

Start each case from this sample set unless its preconditions specify a variant. Reset between cases and between independent attempts that require the same starting state. Fixtures containing historical Sales already reflect those Sales in opening stock. Verify record counts and persisted values using authorized test environment evidence where a role cannot view them. Fixture labels stand for the corresponding system-assigned identifiers where applicable.

| SKU | Name | Category | Unit | Retail price | Wholesale price | Quantity on hand | Reorder level | Active status |
| --- | --- | --- | --- | ---: | ---: | ---: | ---: | --- |
| CBL-01 | Sample Cable | Cables | each | G$2,000 | G$1,000 | 5 | 2 | Active |
| DCK-01 | Sample Dock | Docks | each | G$6,000 | G$5,000 | 12 | 2 | Active |
| ADP-01 | Sample Adapter | Adapters | each | G$3,000 | G$2,000 | 1 | 1 | Active |
| SPL-01 | Sample Splitter | Adapters | each | G$1,000 | G$500 | 0 | 1 | Inactive |
| HUB-01 | Sample Hub | Hubs | each | Pending initial approval | Pending initial approval | 0 | 0 | Active when created |

HUB-01 is absent at baseline. Its proposed initial retail and wholesale prices are G$3,000 and G$2,000. The proposed replacement retail price for CBL-01 is G$2,500. Neither proposal exists at baseline.

| Customer fixture | Name | Type | Contact details | Starting balance owed |
| --- | --- | --- | --- | --- | ---: |
| C1 | Sample Finch | Retail | finch@example.invalid | G$8,500 |
| C2 | Sample Kestrel | Wholesale | kestrel@example.invalid | G$2,000 |
| C3 | Sample Tern | Retail | tern@example.invalid | G$0 when created |

C3 is absent at baseline. For the C1 edit, use the name Sample Finch Annex, type wholesale and finch-annex@example.invalid.

| Sale | Invoice number | Sale and issue date | Customer | Completed Sale Items | Calculated total | Recorded payment | Amount owed | Payment status |
| --- | ---: | --- | --- | --- | ---: | ---: | ---: | --- |
| S1 | 101 | October 1, 2026 | C1 | 2 × CBL-01 at G$2,000 | G$4,000 | G$1,500 | G$2,500 | partly paid |
| S2 | 102 | October 1, 2026 | C1 | 1 × DCK-01 at G$6,000 | G$6,000 | G$0, no Payment | G$6,000 | unpaid |
| S3 | 103 | October 2, 2026 | C2 | 1 × CBL-01 at G$1,000 | G$1,000 | G$1,000 | G$0 | paid |
| S4 | 104 | October 3, 2026 | C2 | 1 × ADP-01 at G$2,000 | G$2,000 | G$0, no Payment | G$2,000 | unpaid |

The next Invoice number is 105. Each historical Payment is dated on its Invoice issue date and attributed to the Sales Clerk. Use October 4, 2026 for new transactions, with timestamps from the controlled test clock. For Invoice 102, the sample partial payment is G$1,500. For Invoice 101, use G$2,500 to settle, G$2,500.01 for overpayment, or G$1,500 for each concurrent payment attempt.

| Staff sign-in identifier | Staff name | Account assignment |
| --- | --- | --- |
| sample-owner-a | Alina Veylan | Owner and Manager, Owner role with System Administrator permissions |
| sample-owner-b | Sample Owner B | Second Owner and Manager account, enabled and available only where stated |
| sample-sales-a | Devren Marlowe | Sales Clerk |
| sample-sales-b | Sample Sales Clerk B | Second Sales Clerk, present only where stated |
| sample-stock-a | Maris Fenwick | Inventory Clerk |

Use distinct User IDs. Where needed, use `Sample-Staff14` (14 characters) and `Sample-Staff15!` (15 characters) as fictional password candidates in the test environment. Use a distinct valid password for each account except in the password storage case.

Use the October 4 delivery note for CBL-01 with expected quantity 3. Use a count of 3 for a match or 2 for a mismatch, with the note “One unit short on count.” Use the same verified quantity of 3 for the SPL-01 inactive variant. For DCK-01 adjustments, use −2 or −3 with reason code damage. For CBL-01 inventory changes, use count correction or the verified receipt specified by the case.

## Product catalogue

### TC-01 Create a Product

**Requirement** FR-01, US-01  
**Preconditions** Owner and Manager is signed in. HUB-01 is unused and has no reconciled opening stock.

**Steps**

1. In Product Record, enter HUB-01, Sample Hub, Hubs, each, reorder level 0 and active status Active. Save.
2. Reopen the Product Record.

**Expected result** One Product exists with the submitted catalogue details, quantity on hand 0 and both approved price fields empty. Its SKU and quantity on hand are read only.  
**Priority** High

### TC-02 Reject a duplicate SKU

**Requirement** FR-02, US-01  
**Preconditions** Owner and Manager is signed in. CBL-01 exists.

**Steps**

1. Create a Product using CBL-01 and the other Sample Hub details. Save.
2. Reopen the original CBL-01 record.

**Expected result** “That SKU already exists. Open the existing product or enter a different SKU.” No duplicate is created. The original Sample Cable details, stock and approved prices remain as seeded.  
**Priority** High

### TC-03 Edit catalogue details

**Requirement** FR-03, US-01  
**Preconditions** Owner and Manager is signed in.

**Steps**

1. Set SPL-01 reorder level to 0 and active status to Active. Save and reopen.
2. Change active status back to Inactive. Save and reopen.

**Expected result** The saved reorder level is 0 and each active status change persists. The SKU, stock and approved prices retain their original values.  
**Priority** High

### TC-04 Hold proposed prices and apply approved prices

**Requirement** FR-04, FR-05, US-02  
**Preconditions** Both Owner accounts are enabled and available. HUB-01 exists with 1 reconciled unit and no approved prices. CBL-01 has its baseline prices.

**Steps**

1. As sample-owner-a, propose the initial HUB-01 prices and the CBL-01 replacement retail price from the sample set.
2. As Sales Clerk, compare both Products in Product List and in new retail sale lines where a price is available.
3. As sample-owner-b, approve the proposed prices. As Sales Clerk, start fresh sale lines for 1 unit of each Product for C1 and again for C2.

**Expected result** Before approval, HUB-01 has no approved prices and CBL-01 still supplies G$2,000 retail and G$1,000 wholesale. After approval, HUB-01 supplies G$3,000 for C1 and G$2,000 for C2. CBL-01 supplies G$2,500 for C1 and G$1,000 for C2.  
**Priority** High

## Stock receiving

### TC-05 Post a verified Supplier Delivery once

**Requirement** FR-06, US-03  
**Preconditions** Inventory Clerk is signed in. No receipt for the sample delivery note has been posted.

**Steps**

1. Count 3 units of CBL-01 against the expected 3 on the note. In Stock Receiving, enter quantity received 3 and receipt date October 4, 2026, and submit the verified receipt.
2. Review the resulting Supplier Delivery. Exercise repeat confirmation for that same receipt through the posting check.

**Expected result** One posted Supplier Delivery has a unique Supplier Delivery ID, CBL-01, receipt date October 4, 2026 and quantity received 3. Quantity on hand is 5 + 3 = 8 after posting and remains 8 on repeat for the same receipt.  
**Priority** High

### TC-06 Hold a mismatch until the counted quantity is confirmed

**Requirement** FR-06, FR-07, US-03  
**Preconditions** Inventory Clerk is signed in. CBL-01 quantity on hand is 5.

**Steps**

1. Record expected quantity 3, counted quantity 2, the sample mismatch note and the receipt date for CBL-01.
2. Check the held receipt and CBL-01 quantity on hand before confirming.
3. Reopen the held receipt and confirm 2 as quantity received.

**Expected result** At the mismatch the system shows “The counts differ. Record the expected quantity, counted quantity and a note, then confirm the counted quantity before posting.” It retains both counts and the note, with Receipt state held for confirmation and stock 5. Confirmation posts that receipt for 2 units, setting stock to 5 + 2 = 7.  
**Priority** High

### TC-07 Block inactive and nonpositive receipts

**Requirement** FR-06, FR-08, US-03  
**Preconditions** Inventory Clerk is signed in. SPL-01 is inactive with 0 units. CBL-01 is active with 5 units.

**Steps**

1. Attempt to post the sample quantity received of 3 for SPL-01.
2. Separately attempt to post quantity received 0 for CBL-01.

**Expected result** Neither attempt posts a receipt. SPL-01 remains at 0 and CBL-01 at 5. For quantity received 0, show “Enter a quantity received greater than zero.”  
**Priority** High

## Stock adjustments

### TC-08 Require a recognized reason code

**Requirement** FR-09, US-04  
**Preconditions** Inventory Clerk is signed in. DCK-01 quantity on hand is 12.

**Steps**

1. Attempt to save a DCK-01 adjustment of −2 with no reason code.
2. Verify separately at server validation that an unrecognized reason such as inspection is rejected for the same adjustment.

**Expected result** Each attempt returns “Choose a reason from the list before saving the adjustment.” Neither posts a Stock Adjustment. Quantity on hand remains 12.  
**Priority** High

### TC-09 Post at exactly G$10,000

**Requirement** FR-10, US-04, US-05  
**Preconditions** Inventory Clerk is signed in. DCK-01 has 12 units and an approved wholesale price of G$5,000.

**Steps**

1. In Stock Adjustment, submit quantity change −2 and reason code damage for DCK-01. The expected Adjustment value is 2 × G$5,000 = G$10,000.
2. Review the saved adjustment. Exercise repeat submission of that same record through the posting check.

**Expected result** The system posts immediately without Owner and Manager approval. One Stock Adjustment records its unique ID, SKU, −2, damage, recorder User ID, adjustment date and time, G$10,000 and an Approval state showing approval is unnecessary. Quantity on hand is 12 − 2 = 10 and remains 10 on repeat.  
**Priority** High

### TC-10 Hold above G$10,000 and post once after approval

**Requirement** FR-11, FR-12, US-05  
**Preconditions** Inventory Clerk can record and Owner and Manager can approve. DCK-01 has 12 units at an approved wholesale price of G$5,000.

**Steps**

1. As Inventory Clerk, submit −3 with reason code damage for DCK-01. The expected Adjustment value is 3 × G$5,000 = G$15,000.
2. Verify the pending Stock Adjustment and quantity on hand.
3. As Owner and Manager, approve the pending adjustment. Exercise repeat processing of that same approval through the posting check.

**Expected result** The G$15,000 adjustment is pending before approval and stock stays 12. Approval is recorded and posts the adjustment once, leaving 12 − 3 = 9 units.  
**Priority** High

### TC-11 Block an adjustment that would make stock negative

**Requirement** FR-13, US-04  
**Preconditions** Inventory Clerk is signed in. CBL-01 has 5 units and an approved wholesale price of G$1,000.

**Steps**

1. Submit a count correction of −6 for CBL-01. The Adjustment value would be 6 × G$1,000 = G$6,000 and the resulting stock would be 5 − 6 = −1.

**Expected result** “This adjustment would bring stock below zero. Check the current quantity on hand and correct the change.” No adjustment posts and quantity on hand remains 5.  
**Priority** High

### TC-12 Recheck stock when approving a held adjustment

**Requirement** FR-11, FR-12, FR-13, US-04, US-05  
**Preconditions** A DCK-01 adjustment of −3, valued at 3 × G$5,000 = G$15,000, is pending from the Inventory Clerk. DCK-01 stock is 12.

**Steps**

1. As Sales Clerk, complete a sale of 10 DCK-01 units for C2 at G$5,000 each, total G$50,000.
2. As Owner and Manager, attempt to approve and post the pending −3 adjustment when stock is 12 − 10 = 2.

**Expected result** Posting would yield 2 − 3 = −1 and is blocked with “This adjustment would bring stock below zero. Check the current quantity on hand and correct the change.” Stock stays at 2.  
**Priority** High

### TC-13 Hold posting without the wholesale valuation input

**Requirement** FR-10, FR-11, US-04, US-05  
**Preconditions** Inventory Clerk is signed in. HUB-01 exists with zero stock and no approved wholesale price.

**Steps**

1. Submit +1 for HUB-01 with reason code count correction while the wholesale price is unavailable.

**Expected result** “An approved wholesale price is needed to check this adjustment. Ask the Owner and Manager to confirm the price.” Posting is held and quantity on hand remains 0.  
**Priority** High

## Customer records

### TC-14 Create a Customer and select them for a sale

**Requirement** FR-14, US-06  
**Preconditions** Sales Clerk is signed in. C3 is absent.

**Steps**

1. In Customer Record, save Sample Tern with type retail and tern@example.invalid.
2. Find the Customer in Customer List and select them in Create Sale.

**Expected result** The saved Customer has a unique Customer ID, the submitted name, type and contact details, and balance owed G$0. Create Sale shows the same Customer ID, name, type and balance owed.  
**Priority** High

### TC-15 Edit a Customer with an outstanding balance

**Requirement** FR-15, US-06  
**Preconditions** Sales Clerk is signed in. C1 owes G$2,500 on Invoice 101 and G$6,000 on Invoice 102.

**Steps**

1. Save the sample C1 edit in Customer Record and reopen it.

**Expected result** The same Customer ID has name Sample Finch Annex, type wholesale and contact details finch-annex@example.invalid. The Invoice links are unchanged and balance owed is G$2,500 + G$6,000 = G$8,500.  
**Priority** High

## Sale creation

### TC-16 Complete a retail sale with an existing balance and no counter payment

**Requirement** FR-16, FR-18, FR-19, FR-23, US-07, US-08, US-10  
**Preconditions** Sales Clerk is signed in. C1 owes G$8,500. CBL-01 has 5 units and DCK-01 has 12.

**Steps**

1. Select C1 in Create Sale and check the displayed existing balance.
2. Add 2 CBL-01 at G$2,000 each and 1 DCK-01 at G$6,000. Complete the sale without payment, then repeat the completion request for the same Sale.

**Expected result** Selection shows G$8,500 and the balance does not prevent completion. One completed Sale links to C1 and records the Sales Clerk User ID, sale date and time and two Sale Items with unique IDs, parent Sale ID, SKUs, quantities and those prices. The lines total G$4,000 and G$6,000. One Invoice is issued unpaid for G$10,000. Stock becomes 5 − 2 = 3 and 12 − 1 = 11 in the same posting and remains at those values on repeat.  
**Priority** High

### TC-17 Charge the approved wholesale price

**Requirement** FR-16, FR-19, US-07, US-08  
**Preconditions** Sales Clerk is signed in. C2 is wholesale and CBL-01 has both approved prices.

**Steps**

1. Select C2, add 2 CBL-01 units and complete the sale.
2. Open the resulting Invoice.

**Expected result** The Sale Item and Invoice show quantity 2 at the G$1,000 wholesale unit price. The line amount and Invoice total both equal 2 × G$1,000 = G$2,000.  
**Priority** High

### TC-18 Block missing sale details and invalid quantities

**Requirement** FR-17, US-07  
**Preconditions** Sales Clerk is signed in. CBL-01 has 5 units.

**Steps**

1. Attempt to complete separately with no Customer and 1 CBL-01, with C1 and no lines, and with C1 and one CBL-01 line set to 0, −1 or 1.5.

**Expected result** Each attempt identifies the invalid or missing input and leaves the sale incomplete, with no Invoice issued and no stock deducted. For 0 and −1, show “Enter a quantity greater than zero.”  
**Priority** High

### TC-19 Block completion when the applicable price is missing or stale

**Requirement** FR-20, US-08  
**Preconditions** Sales Clerk and an eligible Owner and Manager can sign in. HUB-01 has 1 reconciled unit and no approved prices. CBL-01 has the sample replacement retail proposal pending.

**Steps**

1. As Sales Clerk, attempt to complete a C1 sale for 1 HUB-01.
2. Start a fresh C1 sale for 1 CBL-01 at the existing G$2,000 retail price.
3. As Owner and Manager, approve the proposed G$2,500 replacement under rule 9. As Sales Clerk, attempt to complete the draft holding G$2,000.

**Expected result** Neither attempt completes, deducts stock or issues an Invoice. Show “An approved price is required for HUB-01. Refresh the line or ask the Owner and Manager to check the price.” and, for the stale line, “An approved price is required for CBL-01. Refresh the line or ask the Owner and Manager to check the price.”  
**Priority** High

### TC-20 Preserve an issued sale's unit price after a price change

**Requirement** FR-21, US-08, US-11  
**Preconditions** Invoice 101 is issued. CBL-01 has the sample proposed replacement retail price. An eligible Owner and Manager and the Sales Clerk can sign in.

**Steps**

1. As Owner and Manager, approve the proposed G$2,500 CBL-01 retail price under rule 9.
2. As Sales Clerk, compare a new C1 line with the Sale Item and Invoice for S1.

**Expected result** The new line uses G$2,500 per unit. S1 still shows 2 units at G$2,000 each, line amount and Invoice total 2 × G$2,000 = G$4,000. Invoice 101 retains its number and amount owed of G$4,000 − G$1,500 = G$2,500.  
**Priority** High

### TC-21 Reject a sale one unit above stock and allow the last unit

**Requirement** FR-22, FR-23, US-09, US-10  
**Preconditions** Sales Clerk is signed in. ADP-01 has 1 unit.

**Steps**

1. Select C1, add one ADP-01 line with quantity 2 and attempt completion.
2. Verify stock before changing the affected line to 1. Retry completion without payment.

**Expected result** Quantity 2 is one above stock. The line is identified and the message is “Only 1 units of ADP-01 are available. Reduce the total quantity for this product or remove its lines.” Stock remains 1 and no Invoice is issued. Quantity 1 equals available stock, so the retry completes, issues one unpaid Invoice for 1 × G$3,000 = G$3,000, and leaves 1 − 1 = 0 units.  
**Priority** High

### TC-22 Combine quantities across lines and recheck on retry

**Requirement** FR-22, FR-23, US-09, US-10  
**Preconditions** Sales Clerk and Inventory Clerk can sign in. CBL-01 has 5 units.

**Steps**

1. As Sales Clerk, select C1 and attempt to complete a sale with two CBL-01 lines of 3 units each, totaling 6 against 5.
2. As Inventory Clerk, post the verified CBL-01 receipt of 3 units, raising stock to 5 + 3 = 8.
3. As Sales Clerk, retry the same draft with both lines unchanged.

**Expected result** The first completion identifies both lines and shows “Only 5 units of CBL-01 are available. Reduce the total quantity for this product or remove its lines.” It posts no sale, Invoice or deduction. The retry uses current stock of 8, completes with stock 8 − 6 = 2, and issues one Invoice totaling 6 × G$2,000 = G$12,000.  
**Priority** High

## Invoice generation

### TC-23 Issue itemized Invoices with the next sequential numbers

**Requirement** FR-24, FR-25, US-11  
**Preconditions** Sales Clerk is signed in. The last issued Invoice is 104.

**Steps**

1. Complete a C1 sale for 2 CBL-01 at G$2,000 and 1 DCK-01 at G$6,000. Open its Invoice.
2. Complete a fresh C2 sale for 1 CBL-01 at G$1,000 and open its Invoice.

**Expected result** The first sale has exactly one linked Invoice, 105, with issue date, recorded sale lines of G$4,000 and G$6,000, calculated total G$10,000 and unpaid status. The second has exactly one linked Invoice, 106, totaling 1 × G$1,000 = G$1,000. Each Invoice omits a separate tax line.  
**Priority** High

### TC-24 Reject incomplete invoicing without consuming an Invoice number

**Requirement** FR-24, FR-26, US-11, US-12  
**Preconditions** Sales Clerk is signed in. The last issued Invoice is 104. The invoicing check can exercise an incomplete C1 draft and a test fixture with no completed Sale Items.

**Steps**

1. Request invoicing and the printable Invoice for the incomplete draft and, separately, request invoicing for the fixture with no completed Sale Items. Abandon the draft.
2. Complete a fresh C1 sale for 1 CBL-01 at G$2,000.

**Expected result** The requests issue no Invoice, consume no Invoice number and provide no printable Invoice for the draft. Stock is unchanged by the rejected requests and abandoned draft. The valid sale issues Invoice 105 for G$2,000.  
**Priority** High

### TC-25 Preserve the sequence across concurrent Invoice issues

**Requirement** FR-24, US-11  
**Preconditions** Two Sales Clerk accounts are signed in. The next Invoice number is 105.

**Steps**

1. Complete two sales concurrently, one for C1 with 1 CBL-01 at G$2,000 and one for C2 with 1 DCK-01 at G$5,000.
2. Compare their Invoice links and numbers.

**Expected result** The two sales issue one Invoice each, using 105 and 106 exactly once each. The C1 Invoice totals G$2,000 and the C2 Invoice totals G$5,000. Each assigned number remains with its Sale.  
**Priority** High

### TC-26 Reopen and print with the original number and current settlement

**Requirement** FR-27, FR-28, US-11, US-12, US-15  
**Preconditions** Sales Clerk is signed in. Invoice 102 exists for S2, unpaid with G$6,000 owed.

**Steps**

1. Request the Invoice again for S2, open Invoice View and Print and print.
2. Record the sample G$1,500 payment for Invoice 102. Reopen the Invoice and print again.

**Expected result** Both prints show Invoice 102, its original issue date, S2, C1's identifiers and name, contact details when needed, and the line for 1 Sample Dock (DCK-01) at G$6,000 with line amount and total G$6,000. The first shows unpaid and G$6,000 owed. The second shows partly paid and G$6,000 − G$1,500 = G$4,500 owed. Both are legible on standard paper. S2 still has one Invoice.  
**Priority** High

## Payments and balances

### TC-27 Set the balance and status when an Invoice is issued

**Requirement** FR-31, FR-32, FR-33, US-14  
**Preconditions** Sales Clerk is signed in. C1 owes G$8,500.

**Steps**

1. Complete a C1 sale for 1 CBL-01 at G$2,000 without recording a payment. Open Invoice 105 and then Customer Record for C1.

**Expected result** Invoice 105 shows unpaid, an empty payment history and G$2,000 − G$0 = G$2,000 owed. C1's balance owed is G$2,500 + G$6,000 + G$2,000 = G$10,500.  
**Priority** High

### TC-28 Record a partial payment

**Requirement** FR-29, FR-31, FR-32, FR-33, US-13, US-14  
**Preconditions** Sales Clerk is signed in. Invoice 102 is unpaid for G$6,000.

**Steps**

1. In Invoice View and Print, record G$1,500 for Invoice 102.
2. Review the saved Payment, Invoice, C1 balance and DCK-01 stock.

**Expected result** One Payment shows a unique Payment ID, Invoice number 102, recorded by User ID for the Sales Clerk, amount G$1,500 and the payment date and time. Invoice 102 becomes partly paid with G$6,000 − G$1,500 = G$4,500 owed. C1's balance is G$2,500 + G$4,500 = G$7,000. Invoice 101 and DCK-01 stock remain at G$2,500 owed and 12 units, respectively.  
**Priority** High

### TC-29 Accept a payment equal to the current balance

**Requirement** FR-29, FR-31, FR-32, FR-33, US-13, US-14  
**Preconditions** Sales Clerk is signed in. Invoice 101 totals G$4,000 and has one Payment of G$1,500.

**Steps**

1. Record G$2,500 against Invoice 101, exactly its current balance. Review the Invoice and C1's Customer Record.

**Expected result** One G$2,500 Payment is added to Invoice 101. Amount owed becomes G$4,000 − (G$1,500 + G$2,500) = G$0 and the Invoice becomes paid. C1 owes G$6,000 from Invoice 102 alone.  
**Priority** High

### TC-30 Reject a payment one cent above the current balance

**Requirement** FR-30, US-13  
**Preconditions** Sales Clerk is signed in. Invoice 101 totals G$4,000 and has one Payment of G$1,500.

**Steps**

1. Submit G$2,500.01 against Invoice 101, one cent above G$4,000 − G$1,500 = G$2,500.
2. Check the payment entry, Invoice and C1 balance.

**Expected result** “The amount owed is G$2,500. Enter a payment up to that amount.” The Sales Clerk can enter a corrected amount. No Payment or credit balance is created. Invoice 101 remains partly paid at G$2,500 owed and C1's balance remains G$8,500.  
**Priority** High

### TC-31 Reject missing or invalid Invoice numbers and nonpositive payments

**Requirement** FR-30, US-13  
**Preconditions** Sales Clerk is signed in. Invoice 101 owes G$2,500 and 105 has yet to be issued.

**Steps**

1. Exercise payment validation with G$1,500 and no Invoice number, then with the unissued number 105.
2. Attempt G$0 and −G$1 against Invoice 101 in separate requests.

**Expected result** Every attempt is rejected. For 0 and −1, show “Enter a payment amount greater than zero.” No Payment is recorded and no Invoice balance or payment status changes.  
**Priority** High

### TC-32 Validate concurrent payments against the current balance

**Requirement** FR-29, FR-30, FR-31, FR-32, US-13, US-14  
**Preconditions** Two Sales Clerk accounts are signed in. Invoice 101 has G$2,500 owed.

**Steps**

1. Submit two simultaneous payments of G$1,500 against Invoice 101, one per account. Together they would total G$3,000 against G$2,500 owed.
2. Review the results and Invoice.

**Expected result** Exactly one new G$1,500 Payment is recorded. The second attempt is rejected with “The amount owed is G$1,000. Enter a payment up to that amount.” Invoice 101 stays partly paid with G$2,500 − G$1,500 = G$1,000 owed and C1 owes G$1,000 + G$6,000 = G$7,000.  
**Priority** High

### TC-33 Show Customer-specific history and retain settled records

**Requirement** FR-34, FR-35, US-15  
**Preconditions** Sales Clerk is signed in. The baseline Payment records exist for Invoices 101 and 103.

**Steps**

1. Record G$2,500 for Invoice 101 to settle it and G$1,500 for Invoice 102 to leave it partly paid.
2. Later in the Release 1 test period, reopen C1 payment history, then open Invoice 101 from it. Review C2 payment history separately.

**Expected result** C1 history shows the G$1,500 and G$2,500 Payments for paid Invoice 101 and the G$1,500 Payment for partly paid Invoice 102, each with Invoice number and its recorded payment date and time. Invoice 101 and its two Payments remain accessible. C2 history shows its own G$1,000 Payment for paid Invoice 103, which likewise remains accessible. Neither history includes the other Customer's Payments.  
**Priority** High

## Receivables

### TC-34 Order open Invoices and reflect partial and full settlement

**Requirement** FR-36, US-16  
**Preconditions** Owner and Manager and Sales Clerk can sign in separately. Invoices 101 through 104 exist as seeded.

**Steps**

1. As Owner and Manager, open Receivables List and verify its displayed order, statuses and amounts.
2. As Sales Clerk, record G$1,500 against Invoice 102. As Owner and Manager, refresh the list that business day.
3. As Sales Clerk, record G$2,500 against Invoice 101. As Owner and Manager, refresh the list that business day.

**Expected result** Initially the list shows 101, 102, 104, in that order, with G$2,500 partly paid, G$6,000 unpaid and G$2,000 unpaid, respectively. Each row shows its Invoice number, issue date, Customer name and age based on issue date. Paid Invoice 103 is absent. After step 2, Invoice 102 stays in position with G$6,000 − G$1,500 = G$4,500 owed and partly paid status. After step 3, Invoice 101 owes G$2,500 − G$2,500 = G$0 and leaves the list, which then shows 102 and 104 in that order.  
**Priority** High

## Low stock

### TC-35 Include stock at the reorder level and remove it above the level

**Requirement** FR-37, US-17  
**Preconditions** All three staff roles can sign in. CBL-01 starts at 5 units with reorder level 2.

**Steps**

1. As Owner and Manager, open Low Stock List.
2. As Sales Clerk, sell 3 CBL-01 units to C1 at G$2,000 each. As Owner and Manager, refresh the list.
3. As Inventory Clerk, post the verified sample receipt for 3 CBL-01 units. As Owner and Manager, refresh again.

**Expected result** Initially ADP-01 appears at 1 against reorder level 1 and SPL-01 at 0 against reorder level 1 under rule 10. CBL-01 and DCK-01 are absent. After the G$6,000 sale, CBL-01 appears at 5 − 3 = 2 against reorder level 2 with its SKU and name. After the receipt it leaves the list at 2 + 3 = 5.  
**Priority** High

## Reporting

### TC-36 Total completed sales within the inclusive period

**Requirement** FR-38, US-18  
**Preconditions** Owner and Manager is signed in. S1 through S4 are complete. An incomplete C1 draft dated October 2 contains 1 CBL-01 at G$2,000.

**Steps**

1. Run Sales by period for October 1 through October 2, 2026, including S1 and S2 on the start date and S3 on the end date.

**Expected result** Period sales total is G$4,000 + G$6,000 + G$1,000 = G$11,000. The report excludes S4 (G$2,000 on October 3) and the incomplete G$2,000 draft.  
**Priority** High

### TC-37 Aggregate completed Sale Items by SKU

**Requirement** FR-39, US-18  
**Preconditions** Owner and Manager is signed in. S1 through S4 are complete. An incomplete C1 draft includes 1 CBL-01 at G$2,000.

**Steps**

1. Run Sales by product. Compare its quantities and sales amounts with the four completed Sales in the sample set.

**Expected result** CBL-01 shows 2 + 1 = 3 units and (2 × G$2,000) + (1 × G$1,000) = G$5,000. DCK-01 shows 1 unit and 1 × G$6,000 = G$6,000. ADP-01 shows 1 unit and 1 × G$2,000 = G$2,000. The incomplete draft contributes zero units and G$0.  
**Priority** High

### TC-38 Withhold stock on hand value until ready, then reconcile it

**Requirement** FR-40, US-19  
**Preconditions** Owner and Manager can sign in and Inventory Clerk can receive stock. Use an unconfirmed setting for the first variant. The other variants require a business-confirmed basis and either missing or complete valuation inputs as stated.

**Steps**

1. As Owner and Manager, run Stock on hand value without a confirmed basis, and separately with a confirmed basis and a required input missing.
2. Once the business confirms the basis and all inputs are available, record the fixture inputs. Independently calculate the G$ value under that basis using the recorded quantities 5, 12, 1 and 0 for CBL-01, DCK-01, ADP-01 and SPL-01, respectively. Run the report.
3. As Inventory Clerk, post the sample receipt for 3 CBL-01. Recompute under the same basis with CBL-01 at 5 + 3 = 8 and current valuation inputs. As Owner and Manager, rerun the report.

**Expected result** No G$ stock value is shown in either missing-setting or missing-input check. With the confirmed basis and required inputs, the report shows the named basis and exactly the independently calculated G$ total for quantities 5, 12, 1 and 0. After the receipt the report shows the same named basis and the independently calculated G$ total for quantities 8, 12, 1 and 0.  
**Priority** High

## Search and filtering

### TC-39 Match Product List search and filters together

**Requirement** FR-41, US-23  
**Preconditions** Sales Clerk is signed in. ADP-01 and SPL-01 share category Adapters.

**Steps**

1. Search for Sample Adapter and filter by category Adapters and active status Active.
2. Switch active status to Inactive and repeat the search. Search for Sample Splitter with those filters still applied.
3. Clear the criteria and search for CBL-01 by SKU.

**Expected result** The first search returns only ADP-01. When the filter changes to Inactive, Sample Adapter returns no rows and Sample Splitter returns only SPL-01. The SKU search returns only CBL-01. Each returned row shows its authorized catalogue, price and stock fields from the sample set.  
**Priority** Medium

### TC-40 Match Customer List search and type together

**Requirement** FR-42, US-23  
**Preconditions** Sales Clerk is signed in. Only C1 and C2 exist.

**Steps**

1. Search Customer List for Sample Kestrel and filter by wholesale. Change the filter to retail and repeat.
2. Clear the criteria and search by C1's Customer ID.

**Expected result** Step 1 returns only C2, wholesale with G$2,000 owed, for the wholesale filter and no records for the retail filter. Step 2 returns only C1, retail with G$8,500 owed. Both rows show the corresponding Customer ID and name.  
**Priority** Medium

### TC-41 Match Sales List search and dates together

**Requirement** FR-43, US-23  
**Preconditions** Sales Clerk is signed in. S1 through S4 are complete.

**Steps**

1. Search Sales List for Sample Finch and filter September 30 through October 2, 2026. Repeat with October 2 through October 4, 2026.
2. Clear the criteria and search by S4's Sale ID.

**Expected result** The first date range returns only S1 and S2 with Invoices 101 and 102. The second date range returns no Sales for Sample Finch. Step 2 returns only S4 with Invoice 104. Each returned row shows its Sale ID, Customer ID and name, creator User ID and sale date and time.  
**Priority** Medium

### TC-42 Match Invoice List search and payment status together

**Requirement** FR-44, US-23  
**Preconditions** Sales Clerk is signed in. Invoices 101 through 104 exist as seeded.

**Steps**

1. Search Invoice List for Sample Finch and filter by unpaid.
2. Clear the criteria and search by Invoice number 103.

**Expected result** Step 1 returns only Invoice 102 with its issue date, C1 name, G$6,000 total and G$6,000 owed. Step 2 returns only Invoice 103 with its issue date, C2 name, G$1,000 total, paid status and G$0 owed.  
**Priority** Medium

## Security

### TC-43 Authenticate individual enabled accounts and attribute actions

**Requirement** SEC-01, US-20, US-21  
**Preconditions** The three baseline accounts are enabled. A separate sample Sales Clerk account is disabled.

**Steps**

1. Sign in with valid credentials as each enabled staff member and check the Dashboard staff name and role. Attempt to sign in separately with the disabled account's correct credentials.
2. As sample-sales-a, sell 1 CBL-01 to C1 at G$2,000 and record G$1,500 against Invoice 102. As sample-stock-a, post the DCK-01 adjustment of −2 valued at 2 × G$5,000 = G$10,000.
3. Compare the recorded creator and recorder User IDs with the authenticated accounts.

**Expected result** Each enabled account has a distinct sign-in identifier and User ID. Dashboard shows the matching sample staff name and current role. The G$2,000 Sale and G$1,500 Payment identify sample-sales-a. The adjustment identifies sample-stock-a and changes DCK-01 stock from 12 to 10. No session starts for the disabled account.  
**Priority** High

### TC-44 Restrict account management and apply role changes on the next request

**Requirement** SEC-02, US-21  
**Preconditions** Owner and Manager and sample-sales-a are signed in. S1 and its Payment identify sample-sales-a. The second Sales Clerk account is absent.

**Steps**

1. As Owner and Manager, create the individual sample-sales-b account with role Sales Clerk. Attempt another account creation with the same sign-in identifier.
2. As sample-sales-a, request User Accounts and access to change a role. As Owner and Manager, change sample-sales-a to Inventory Clerk.
3. In the existing sample-sales-a session, request access to Invoice 101 and Stock Receiving. Verify S1 and its Payment attribution through authorized evidence.

**Expected result** Creation assigns sample-sales-b a distinct User ID and the duplicate identifier is rejected. The Sales Clerk's management requests are denied with “Your account does not have permission for this action. Ask the Owner and Manager if you need access.” Following the role change, the next requests follow Inventory Clerk permissions. Invoice 101 is denied with the same message, while Stock Receiving is allowed. The original User ID remains on S1 and its Payment.  
**Priority** High

### TC-45 Enforce the 15-character password minimum

**Requirement** SEC-03, US-20, US-21  
**Preconditions** Owner and Manager is signed in. sample-sales-b is available for the account setup and credential reset variants.

**Steps**

1. On setup, attempt to save using the 14-character candidate. Repeat with the 15-character candidate.
2. Restore a fixture with sample-sales-b enabled and a separate valid password. Repeat the candidate checks during password reset.

**Expected result** The 14-character candidate is rejected in both workflows. A rejected reset preserves the previous credential. The 15-character candidate satisfies the minimum on setup and reset, and sign-in works with the newly saved credential. The saved password is never displayed.  
**Priority** High

### TC-46 Store passwords as salted hashes

**Requirement** SEC-04, US-21  
**Preconditions** Owner and Manager has two individual Sales Clerk accounts available. Authorized read-only implementation and test storage evidence is available.

**Steps**

1. Set the same fictional 15-character candidate for both Sales Clerk accounts.
2. Inspect how both credentials are written and verified, then inspect the stored representations and their associated salts.

**Expected result** The implementation uses a password hashing function with a separate salt per account. The shared candidate produces different stored salted hashes. Stored credential records contain neither plaintext nor a reversible representation of the password.  
**Priority** High

### TC-47 Lock sign-in after five consecutive failures for 15 minutes

**Requirement** SEC-05, US-20, US-22  
**Preconditions** sample-sales-a is enabled with zero failures. The test clock can measure elapsed time. Use the 14-character candidate as an incorrect password and sample-unknown as the unrecognized sign-in identifier.

**Steps**

1. Submit four failed sign-ins for sample-sales-a, then sign in with valid credentials and sign out.
2. Submit five consecutive failed sign-ins for sample-sales-a. Try the correct credential just before 15 minutes have elapsed since the fifth failure. Repeat at expiry.
3. In a separate lockout run, let 15 minutes elapse without signing in successfully, then submit one failed attempt and immediately try the correct credential. Attempt sign-in separately with sample-unknown.

**Expected result** Valid credentials work after four failures in step 1 and reset the count. The next lockout starts on the fifth consecutive failure. Correct credentials are rejected before expiry and accepted at expiry. Expiry clears the count, so one later failure does not lock the account. Invalid, unknown and locked sign-in attempts all display “Sign-in failed. Check your sign-in identifier and password. If you have tried several times, wait 15 minutes before trying again.”  
**Priority** High

### TC-48 Deny direct access to the Owner-only price change function

**Requirement** SEC-06, US-02, US-20  
**Preconditions** Sales Clerk is signed in. CBL-01 has its baseline approved prices.

**Steps**

1. Open the Owner-only price change function directly for CBL-01 while signed in as the Sales Clerk.
2. Verify the authorization outcome for proposing or approving its sample replacement retail price as the Sales Clerk.

**Expected result** Access and both price actions are denied server-side with “Your account does not have permission for this action. Ask the Owner and Manager if you need access.” No approval details are disclosed, no proposal or approval is saved, and the approved prices remain G$2,000 retail and G$1,000 wholesale.  
**Priority** High

### TC-49 Enforce role, record and field access in protected views and actions

**Requirement** SEC-06, US-20, US-23  
**Preconditions** All three roles can sign in separately. Use the baseline Product, Customer, Sale, Invoice and stock receipt fixtures relevant to each request.

**Steps**

1. As Inventory Clerk, search Product List for CBL-01 and review the returned fields. Request Customer List, Sales List and Invoice List with criteria that match C1, S1 or Invoice 101.
2. As Owner and Manager, request Customer Record and its payment history for C1, Create Sale, Invoice 101 and its print and payment functions, and Stock Receiving.
3. As Sales Clerk, request Stock Receiving, Stock Adjustment, Receivables List, Low Stock List and Reports.

**Expected result** Inventory Clerk sees authorized CBL-01 catalogue and stock fields, with neither approved price returned. All out-of-role requests are denied with “Your account does not have permission for this action. Ask the Owner and Manager if you need access.” The responses disclose no protected records or fields and cause no record changes.  
**Priority** High

### TC-50 Redirect signed-out direct Invoice access to Sign In

**Requirement** SEC-07, US-20  
**Preconditions** Invoice 101 exists. A signed-out staff browser has its direct Invoice address.

**Steps**

1. While signed out, open the direct address of Invoice 101.
2. Verify the result of an unauthenticated payment request for Invoice 101 using the application test interface.

**Expected result** The browser is redirected to Sign In before any Invoice detail is returned. The protected payment request is rejected without exposing Invoice data or recording a Payment.  
**Priority** High

### TC-51 Require HTTPS for content and application input

**Requirement** SEC-08, US-20  
**Preconditions** The built system is available at its test deployment address. A fictional Sales Clerk account and authorized deployment review evidence are available.

**Steps**

1. Verify the HTTP and HTTPS behaviour for Sign In and a protected Invoice screen. Check the deployment policy for accepting credentials and other application input.
2. Sign in and open Invoice 101 over HTTPS.

**Expected result** HTTP serves no application content and accepts no credentials or application input. Sign In, its credential submission and the authorized Invoice response operate over HTTPS.  
**Priority** High

### TC-52 Expire sessions at the configured inactivity limit

**Requirement** SEC-09, US-20  
**Preconditions** The initial inactivity setting is 15 minutes. Owner and Manager and Sales Clerk can sign in. A controlled clock is available.

**Steps**

1. As Sales Clerk in a fresh session, request Invoice 101 just before reaching 15 minutes of inactivity.
2. In a separate fresh session, remain inactive until 15 minutes and request Invoice 101.
3. As Owner and Manager, set the inactivity timeout to 10 minutes. Repeat both checks with fresh Sales Clerk sessions and the new limit.

**Expected result** Requests just before the applicable idle limit are allowed. Requests at the limit are rejected, disclose no Invoice data and require fresh Sign In. Fresh authorized sign-in restores access. The Owner and Manager's saved 10-minute setting governs the new checks.  
**Priority** High

### TC-53 Require current independent authorization when a second approver is available

**Requirement** SEC-10, US-02, US-05  
**Preconditions** Both Owner accounts are available. HUB-01 has no approved price. Eligibility changes can be made through User Accounts in the test environment.

**Steps**

1. As sample-owner-a, propose the initial HUB-01 prices and the G$2,500 CBL-01 replacement. Attempt to approve each proposal while sample-owner-b remains authorized.
2. As sample-owner-b, approve both proposals. In a separate pending proposal variant, keep sample-owner-b signed in and have sample-owner-a change sample-owner-b to Sales Clerk in User Accounts. In that existing sample-owner-b session, request approval.
3. As Inventory Clerk, submit the DCK-01 adjustment of −3 valued at 3 × G$5,000 = G$15,000. Verify that the Inventory Clerk cannot approve it. As a currently authorized Owner and Manager, approve it.

**Expected result** Each independent price approval is required and the proposer's attempts show “Ask another authorized Owner and Manager to approve this change.” Prices remain pending until the eligible second Owner approves. The former Owner session and the Inventory Clerk cannot approve and receive “Your account does not have permission for this action. Ask the Owner and Manager if you need access.” The adjustment leaves stock at 12 pending approval and posts to 9 once authorized. Approval entries identify the proposer or recorder and distinct approver, with the self-approved indicator clear.  
**Priority** High

### TC-54 Audit permitted self-approval with one authorized approver

**Requirement** SEC-10, US-02, US-05  
**Preconditions** sample-owner-a is the only authorized Owner and Manager. CBL-01 and DCK-01 have their baseline values.

**Steps**

1. As sample-owner-a, propose and approve the G$2,500 replacement retail price for CBL-01.
2. As the same Owner and Manager, record a DCK-01 adjustment of −3 with reason code damage, valued at 3 × G$5,000 = G$15,000. Check stock before approving, then approve.
3. Open both approval entries in Audit Trail View.

**Expected result** The approved price becomes G$2,500. The adjustment is held at 12 units until approval, then posts to 12 − 3 = 9. Each approval entry shows sample-owner-a as both proposer or recorder and approver, the changed approval state and a self-approved indicator.  
**Priority** High

### TC-55 End the authenticated session on sign-out

**Requirement** SEC-11, US-20  
**Preconditions** Sales Clerk is signed in and Invoice 101 is open.

**Steps**

1. Sign out, then reopen the protected Invoice address.
2. Verify through the session test interface that the ended session is no longer authorized for protected reads or actions.

**Expected result** Reopening the Invoice leads to Sign In without returning protected data. The ended session authorizes neither protected reads nor actions, and no record changes.  
**Priority** High

### TC-56 Audit a payment, price change, adjustment and sale

**Requirement** SEC-12, US-02, US-04, US-05, US-13, US-22  
**Preconditions** All roles are available and a second Owner and Manager is eligible to approve. A synchronized reference clock and baseline records are available.

**Steps**

1. As Sales Clerk, create and complete a C1 sale for 1 CBL-01 at G$2,000. Record G$1,500 against Invoice 102.
2. As sample-owner-a, propose the CBL-01 G$2,500 retail price. As sample-owner-b, approve it.
3. As Inventory Clerk, post −2 DCK-01 with reason code damage and Adjustment value 2 × G$5,000 = G$10,000.
4. As Owner and Manager, locate the corresponding entries in Audit Trail View and compare them to the recorded actions and reference clock.

**Expected result** Every completed audited action has an associated entry with responsible User ID, synchronized UTC date and time, action, affected identifier, source function and outcome. Sale creation shows the Sale ID and C1 with empty before values. Sale completion shows Invoice 105, the G$2,000 Sale Item and stock 5 to 4. Payment recording identifies the new Payment ID and Invoice 102, amount G$1,500, and the change from unpaid at G$6,000 to partly paid at G$4,500. The price entry shows CBL-01 retail price from G$2,000 to G$2,500, and the approval entry shows both Owner User IDs and its approval state change. The adjustment entry identifies the Stock Adjustment ID, DCK-01, damage, −2, posting state and stock 12 to 10. Entries omit credential values, session secrets and card data.  
**Priority** High

### TC-57 Audit sign-in, session end, permission denial and account changes

**Requirement** SEC-12, US-21, US-22  
**Preconditions** Owner and Manager and Sales Clerk accounts are available, with synchronized UTC reference time.

**Steps**

1. As sample-sales-a, attempt an incorrect password and then sign in with the correct password. Request the Owner-only CBL-01 price function, then sign out.
2. Repeat sign-in in a separate session and let it expire after 15 idle minutes. In a separate run trigger the five-failure lockout. Try the sample-unknown identifier.
3. As Owner and Manager, enable sample-sales-b from the disabled account fixture and reset its credential. Review all relevant Audit Trail View entries.

**Expected result** Entries show UTC date and time, action, source, outcome and User ID where attributable. Sign-in entries record password as authentication method, failed attempts show the attempted sign-in identifier, and the fifth consecutive failure records lockout. Session end entries distinguish sign-out from expiry. Denial identifies the price function and affected SKU when known without protected contents. Account changes identify sample-sales-b's User ID and before and after access values, with the credential reset recorded by event only. Entries contain no credential values, session secrets or card data.  
**Priority** High

### TC-58 Restrict Audit Trail View to the Owner and Manager

**Requirement** SEC-13, US-22  
**Preconditions** An audit entry exists for the historical Payment on Invoice 101. All three roles can sign in separately.

**Steps**

1. As Owner and Manager, open Audit Trail View and review that entry.
2. As each Clerk role, request the same view and entry.

**Expected result** The Owner and Manager can view the entry. Each Clerk request is denied with “Your account does not have permission for this action. Ask the Owner and Manager if you need access.” Neither Clerk receives the entry's metadata or contents.  
**Priority** High

### TC-59 Protect existing audit entries against edits and deletions

**Requirement** SEC-14, US-22  
**Preconditions** An audit entry exists for the historical Payment on Invoice 101. Each staff role can be checked in the authorized test environment.

**Steps**

1. Verify whether each role, including Owner and Manager, can edit or delete that entry through application actions or their server-side authorization checks.
2. As Owner and Manager, review the original entry again.

**Expected result** No role can edit or delete the entry. Any submitted unauthorized action is denied with “Your account does not have permission for this action. Ask the Owner and Manager if you need access.” The original entry remains present with identical values.  
**Priority** High

### TC-60 Enforce validation at the server for forms and search criteria

**Requirement** SEC-15, US-01, US-03, US-04, US-07, US-13, US-23  
**Preconditions** Each authorized staff role has its own session. Server-side validation can be observed through authorized test evidence.

**Steps**

1. As Owner and Manager, check proposed retail prices G$0 and −G$1 for CBL-01 and reorder level −1 for SPL-01.
2. As Inventory Clerk, check expected quantity −1 and counted quantity −1 separately for the CBL-01 mismatch, quantity received −1 for the CBL-01 receipt, and quantity change 0 for a CBL-01 count correction.
3. As Sales Clerk, check sale quantity −1 for CBL-01, payment amount −G$1 for Invoice 101 and a Sales List date range October 3 to October 1, 2026.
4. Check the server's acceptance of the valid signed DCK-01 stock adjustment of −2 with reason code damage.

**Expected result** Server validation rejects the invalid submissions. It shows “Enter a proposed retail price greater than zero.”, “Enter a quantity received greater than zero.”, “Enter a quantity greater than zero.” and “Enter a payment amount greater than zero.” for the respective positive-value checks. The invalid reorder level and receipt count submissions show “Enter zero or a positive number for reorder level.”, “Enter zero or a positive number for expected quantity.” and “Enter zero or a positive number for counted quantity.” The zero adjustment shows “Enter the number of units to add or remove. The change must be greater or less than zero.” The reversed date range is rejected. Invalid requests cause no record changes. The valid adjustment posts at 2 × G$5,000 = G$10,000 and changes DCK-01 stock from 12 to 10.  
**Priority** High

### TC-61 Limit collected Customer information

**Requirement** SEC-16, US-06  
**Preconditions** Sales Clerk is signed in. The C3 fixture and authorized Customer input and persistence evidence are available.

**Steps**

1. Inspect the Customer Record collection fields and save C3 using its sample name, type and contact details.
2. Review the accepted input fields and saved record.

**Expected result** Customer data entry and persistence are limited to name, type and contact details needed for invoicing and contact. The saved record has the sample values, a system-set ID and calculated balance G$0. Other displayed transaction values come from system records.  
**Priority** High

### TC-62 Keep card data out of stored fields

**Requirement** SEC-17, US-06, US-13  
**Preconditions** Authorized test evidence is available for Customer Record, Stock Receiving, Invoice View and Print, and all other Release 1 storage paths with staff-entered fields. The QA environment supplies a marked synthetic card-data fixture under the implemented validation contract.

**Steps**

1. Review field definitions and persisted records for dedicated card fields. Identify all staff-entered fields, including free text such as C3 contact details and the CBL-01 mismatch note.
2. In the controlled test environment, exercise rejection or omission of the synthetic fixture for each applicable input path, including attempts to associate it with the sample Invoice 102 Payment. Inspect persisted business records and audit data after submission.

**Expected result** Release 1 offers no card-data field. The synthetic card-data fixture is absent from persisted fields and audit records, including free text and Payment records. Staff adherence to the free-text handling rule is verified separately during UAT before sign off.  
**Priority** High

### TC-63 Reject a manually supplied Invoice total

**Requirement** SEC-18, US-11  
**Preconditions** Sales Clerk is signed in. A validated C1 Sale draft contains 2 CBL-01 at G$2,000 each and 1 DCK-01 at G$6,000.

**Steps**

1. Verify the Invoice total is system set. Exercise the server validation check for a manually supplied G$6,000 Invoice total for these lines.
2. Complete the valid draft using system calculation.

**Expected result** The manually supplied total is rejected and creates no Invoice at G$6,000. Valid completion yields one Invoice totaling (2 × G$2,000) + (1 × G$6,000) = G$10,000.  
**Priority** High

### TC-64 Record daily backup completion

**Requirement** SEC-19, US-24  
**Preconditions** The sample catalogue, Customers, Sales, Invoices and Payments are present. The test environment runs over two consecutive operational days, October 4 and 5, 2026.

**Steps**

1. Allow the daily backup to run on both days.
2. As Owner and Manager, verify both backup outcomes using the built system's available evidence.

**Expected result** A backup of system data completes on each day, with a dated outcome and verifiable evidence of successful completion for each day.  
**Priority** High

### TC-65 Restore the sample records and reconcile them before sign off

**Requirement** SEC-20, US-24  
**Preconditions** A daily backup contains the baseline sample records. An isolated restore destination, authorized restore operator and Owner and Manager reviewer are available.

**Steps**

1. Restore the backup to the isolated destination. As Owner and Manager, verify the restored stock, Customer records, Sales, Invoices, Payments and balances against the saved fixture.
2. In a separate restore check, use a controlled restore failure or reconciliation discrepancy. Verify the disposition, then repeat with the valid backup and reconcile.

**Expected result** The valid restore recovers stock quantities 5, 12, 1 and 0, C1 and C2 details, S1 through S4 with their Sale Items, Invoices 101 through 104 with balances G$2,500, G$6,000, G$0 and G$2,000, and the G$1,500 and G$1,000 Payments tied to 101 and 103. Restored C1 balance is G$2,500 + G$6,000 = G$8,500 and C2 balance is G$0 + G$2,000 = G$2,000. A failure or discrepancy keeps the restore criterion open until a valid restore is reconciled.  
**Priority** High

### TC-66 Protect the last unit during concurrent stock changes

**Requirement** SEC-21, US-09, US-10  
**Preconditions** Two Sales Clerk accounts and Inventory Clerk can sign in. ADP-01 has 1 unit and retail price G$3,000.

**Steps**

1. As separate Sales Clerks, prepare two C1 sales, each for 1 ADP-01, and attempt to complete both concurrently.
2. Reset ADP-01 and repeat with one C1 sale for 1 ADP-01 concurrent with an Inventory Clerk count correction of −1, valued at 1 × G$2,000 = G$2,000.

**Expected result** In step 1 exactly one sale completes, issuing one G$3,000 Invoice and deducting 1 unit. The other sale is blocked and shows “Only 0 units of ADP-01 are available. Reduce the total quantity for this product or remove its lines.” In step 2 exactly one movement posts. If the adjustment posts first, the sale shows the same insufficient-stock message and issues no Invoice. If the sale posts first, one G$3,000 Invoice is issued and the adjustment shows “This adjustment would bring stock below zero. Check the current quantity on hand and correct the change.” Both variants end with 1 − 1 = 0 units.  
**Priority** High

## Change log

| Version | Date | Change |
| --- | --- | --- |
| 1 | November 24, 2026 | Initial draft of Release 1 functional and security test cases. |
