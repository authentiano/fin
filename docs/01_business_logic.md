# BookRecord System
## Business Logic Documentation

**Version:** 1.0

**Status:** Draft

**Author:** Software Architecture Team

---

# 1. Introduction

## Purpose

This document defines the business rules, workflows, and operational logic of the BookRecord System.

The purpose of this document is to explain **how the business works**, not how the software is programmed.

Every feature implemented in the backend, frontend, and database must conform to the business rules defined here.

---

# 2. Business Overview

The BookRecord System is a web-based management platform designed to help businesses manage:

- Customers
- Customer Orders
- Customer Deposits
- Customer Payments
- Outstanding Balances
- Revenue
- Reports
- User Accounts
- Business Activities

The system acts as a centralized source of truth for all business transactions.

---

# 3. Core Business Objects

The business revolves around the following entities:

Customer

↓

Order

↓

Payment

↓

Deposit

↓

Balance

↓

Reports

Every activity performed within the system affects one or more of these business objects.

---

# 4. Customer Lifecycle

A customer is the foundation of every transaction.

Business Rules

• Every customer must have a unique identifier.

• Customers may have multiple orders.

• Customers may make multiple payments.

• Customers may have multiple deposits.

• Customers may owe outstanding balances.

• Customers cannot be deleted if financial transactions exist.

Customer Lifecycle

New Customer

↓

Customer Registration

↓

Places Order

↓

Makes Deposit

↓

Goods Ordered

↓

Goods Arrive

↓

Customer Pays Balance

↓

Transaction Completed

↓

Customer History Archived

---

# 5. Order Lifecycle

An order represents a request made by a customer.

Business Rules

Every order belongs to one customer.

Every order has one status.

Every order has one supplier.

Every order has a total cost.

Every order contributes to the customer's balance.

Order Status

Draft

↓

Confirmed

↓

Ordered

↓

In Transit

↓

Arrived

↓

Collected

↓

Completed

↓

Archived

Business Constraints

An order cannot be marked Completed until:

• Outstanding balance = 0

AND

• Goods have been collected

---

# 6. Deposit Logic

Deposits are advance payments made before the order is fully paid.

Business Rules

A deposit:

• belongs to one customer

• may be linked to one or many orders

• reduces outstanding balance

• cannot exceed the remaining balance

Formula

Outstanding Balance

=

Order Total

-

Deposit

-

Payments

Example

Order Total = 2,000,000 UGX

Deposit = 500,000 UGX

Payment = 300,000 UGX

Outstanding

=

1,200,000 UGX

---

# 7. Payment Logic

Payments reduce customer debt.

Business Rules

A payment:

belongs to one customer

may pay one order

may pay multiple orders

must have payment method

must have payment date

must have amount

Payment Flow

Customer

↓

Payment Recorded

↓

Allocation

↓

Balance Updated

↓

Receipt Generated

↓

Reports Updated

Payment Methods

Cash

Mobile Money

Bank Transfer

Cheque

Card

Other

---

# 8. Payment Allocation Logic

Sometimes one payment settles multiple orders.

Example

Customer pays

3,000,000 UGX

Orders

Order A

1,000,000

Order B

800,000

Order C

1,200,000

Payment Allocation

Payment

↓

Allocation Table

↓

Order A

↓

Order B

↓

Order C

Remaining Balance

↓

Updated Automatically

Business Rules

Allocated amount cannot exceed payment.

Allocated amount cannot exceed order balance.

---

# 9. Outstanding Balance Logic

Outstanding balance represents money still owed.

Formula

Outstanding

=

Total Orders

-

Deposits

-

Payments

Example

Orders

5,000,000

Deposits

1,000,000

Payments

2,000,000

Outstanding

2,000,000

Business Rules

Outstanding balance cannot become negative.

---

# 10. Customer Statement

Every customer has an account statement.

Contains

Customer Information

Orders

Payments

Deposits

Outstanding Balance

Receipts

Transaction History

Statement is generated on demand.

---

# 11. Dashboard Logic

Dashboard displays real-time business statistics.

KPIs

Today's Revenue

Outstanding Balance

Active Customers

Pending Orders

Completed Orders

Monthly Revenue

Top Customers

Recent Payments

Recent Orders

Charts

Revenue Trend

Orders Trend

Payment Trend

Customer Growth

---

# 12. Reports

Reports summarize business performance.

Types

Revenue Report

Customer Balance Report

Outstanding Orders

Completed Orders

Deposits

Payments

Business Rules

Reports must support:

Date Filters

Customer Filters

Export to PDF

Export to Excel

Printing

---

# 13. User Management

Users access the system based on roles.

Roles

Administrator

Manager

Cashier

Staff

Viewer

Permissions

Create Customer

Edit Customer

Delete Customer

Create Order

Approve Payment

Generate Reports

Manage Users

Business Rule

Permissions are assigned by role.

---

# 14. Audit Trail

Every important action must be logged.

Logged Events

Login

Logout

Create Customer

Update Customer

Delete Customer

Create Order

Edit Order

Record Payment

Delete Payment

Generate Report

Audit Fields

User

Action

Timestamp

Affected Record

Old Value

New Value

---

# 15. Notifications

The system may notify users when:

Goods arrive

Outstanding balance exists

Order completed

Large payment recorded

New customer registered

---

# 16. Business Rules Summary

Customer

✔ Can have many orders

✔ Can have many payments

✔ Can have many deposits

Order

✔ Must belong to customer

✔ Has one status

✔ Has supplier

✔ Has total cost

Deposit

✔ Reduces balance

✔ Cannot exceed balance

Payment

✔ Reduces debt

✔ Can pay multiple orders

Outstanding Balance

✔ Never negative

Reports

✔ Real-time

✔ Printable

✔ Exportable

Audit

✔ Every critical action logged

---

# 17. Future Business Expansion

Future versions may include:

Inventory Management

Supplier Management

Purchase Orders

Expense Tracking

Accounting

SMS Notifications

Email Notifications

Barcode Scanning

QR Code Receipts

Multi-Branch Support

Offline Mode

Mobile Application

Artificial Intelligence Analytics

---

# 18. Conclusion

The business logic described in this document forms the foundation of the BookRecord System.

All database tables, backend APIs, frontend pages, reports, and user interfaces should be implemented according to these rules.

No implementation should violate the business constraints documented here.

This document serves as the authoritative reference for developers, testers, designers, and stakeholders throughout the software development lifecycle.