# BookRecord System
# Functional Requirements Specification (FRS)

Version: 1.0

Status: Draft

Author: Software Architecture Team

---

# Table of Contents

1. Introduction
2. Functional Overview
3. User Roles
4. Authentication Module
5. Dashboard Module
6. Customer Management Module
7. Order Management Module
8. Deposit Management Module
9. Payment Management Module
10. Balance Management Module
11. Reports Module
12. User Management Module
13. Audit Trail Module
14. Notification Module
15. Settings Module
16. Search Module
17. File Management
18. Export Module
19. System Administration
20. Future Functional Requirements

---

# 1. Introduction

## Purpose

This document specifies all functional requirements for the BookRecord System.

Functional requirements describe **what the system must do** from the user's perspective.

These requirements serve as the foundation for database design, API development, frontend implementation, and system testing.

---

# 2. Functional Overview

The system shall provide the following major functions:

- User Authentication
- Customer Management
- Order Management
- Deposit Management
- Payment Processing
- Balance Tracking
- Dashboard Analytics
- Report Generation
- User Administration
- Audit Logging
- Notifications
- Search
- Data Export
- System Configuration

---

# 3. User Roles

The system shall support multiple user roles.

## Administrator

The Administrator shall be able to:

- Create users
- Edit users
- Delete users
- Reset passwords
- Assign roles
- Configure system settings
- View reports
- Manage permissions
- View audit logs

---

## Manager

The Manager shall be able to:

- View dashboard
- Manage customers
- Manage orders
- Record payments
- Approve transactions
- Generate reports

---

## Staff

The Staff shall be able to:

- Register customers
- Create orders
- Record payments
- View customer history

---

## Viewer

The Viewer shall be able to:

- View reports
- View customers
- View dashboard

The Viewer shall not modify any data.

---

# 4. Authentication Module

The system shall:

- Allow users to log in.
- Allow users to log out.
- Authenticate using username and password.
- Support JWT authentication.
- Support refresh tokens.
- Support password hashing.
- Lock accounts after multiple failed login attempts.
- Allow password reset.
- Record login history.
- Record logout history.

---

# 5. Dashboard Module

The dashboard shall display:

- Total Customers
- Active Customers
- Total Orders
- Orders Awaiting Arrival
- Orders Completed
- Total Revenue
- Outstanding Balance
- Total Deposits
- Payments Received Today
- Monthly Revenue
- Top Customers
- Recent Payments
- Recent Orders
- Business Announcements

The dashboard shall refresh automatically.

---

# 6. Customer Management Module

The system shall allow users to:

Create Customer

Edit Customer

Delete Customer

Archive Customer

Restore Customer

Search Customer

Filter Customers

View Customer Details

View Customer Orders

View Customer Payments

View Customer Deposits

View Customer Statements

View Customer Balance

Each customer shall have:

- Unique Customer Number
- First Name
- Last Name
- Phone Number
- Email
- Physical Address
- Registration Date
- Status

---

# 7. Order Management Module

The system shall allow users to:

Create Order

Update Order

Cancel Order

Delete Order

Archive Order

Search Orders

Filter Orders

View Order Details

Assign Supplier

Assign Country

Track Shipment

Update Arrival Date

Mark Goods Received

Mark Goods Collected

Complete Order

Each order shall include:

Order Number

Customer

Supplier

Country

Description

Quantity

Weight

Order Value

Deposit

Outstanding Balance

Arrival Date

Status

Created Date

---

# 8. Deposit Management Module

The system shall:

Record deposits.

Edit deposits.

Cancel deposits.

View deposit history.

Allocate deposits.

Calculate outstanding balances.

Validate deposit amounts.

Generate deposit receipts.

Prevent deposits exceeding balance.

---

# 9. Payment Management Module

The system shall:

Record payments.

Edit payments.

Delete payments.

Allocate payments.

Support partial payments.

Support full payments.

Support payment splitting.

Generate receipts.

Print receipts.

Email receipts.

Accept payment methods:

Cash

Mobile Money

Bank Transfer

Cheque

Card

Other

---

# 10. Balance Management Module

The system shall automatically calculate:

Outstanding Balance

Customer Balance

Order Balance

Deposit Balance

Payment Balance

Balance Formula

Outstanding Balance

=

Order Total

-

Deposits

-

Payments

The balance shall update automatically.

Negative balances shall not be allowed.

---

# 11. Reports Module

The system shall generate:

Revenue Report

Outstanding Balance Report

Customer Report

Order Report

Payment Report

Deposit Report

Monthly Revenue Report

Daily Revenue Report

Annual Revenue Report

Top Customer Report

Inactive Customer Report

Reports shall support:

Date Filters

Customer Filters

Supplier Filters

Status Filters

PDF Export

Excel Export

Printing

---

# 12. User Management Module

The system shall:

Create users.

Edit users.

Delete users.

Deactivate users.

Reset passwords.

Assign roles.

Manage permissions.

View login history.

Lock accounts.

Unlock accounts.

---

# 13. Audit Trail Module

The system shall log:

Login

Logout

Customer Creation

Customer Update

Customer Deletion

Order Creation

Order Update

Order Cancellation

Payment Recording

Deposit Recording

User Creation

Role Assignment

Settings Changes

Each audit log shall contain:

User

Date

Time

Action

Affected Record

Previous Value

New Value

IP Address

---

# 14. Notification Module

The system shall notify users when:

Goods arrive.

Outstanding balance exists.

Large payment is recorded.

New customer registers.

Order is completed.

Password changes.

Low stock (future).

Notifications may be:

Dashboard

Email

SMS (future)

Push Notification (future)

---

# 15. Settings Module

The Administrator shall configure:

Company Name

Logo

Currency

Business Address

Phone Number

Email

Tax Rate

Receipt Footer

Default Country

Default Supplier

Theme

System Backup

---

# 16. Search Module

The system shall provide global search.

Users shall search by:

Customer Name

Customer Number

Phone

Email

Order Number

Supplier

Country

Payment Reference

Receipt Number

Search results shall be displayed instantly.

---

# 17. File Management

The system shall allow uploading:

Customer Documents

Receipts

Invoices

Shipping Documents

Contracts

Images

Each uploaded file shall have:

Owner

Upload Date

Size

File Type

Download Link

---

# 18. Export Module

Users shall export data as:

PDF

Excel

CSV

Print

Exports shall preserve filters.

---

# 19. System Administration

The system administrator shall:

Backup database.

Restore database.

Manage users.

Configure permissions.

Manage application settings.

View logs.

Monitor system activity.

---

# 20. Future Functional Requirements

Future versions may include:

Inventory Management

Supplier Portal

Purchase Orders

Expense Management

Accounting Integration

QR Code Receipts

Barcode Scanner

WhatsApp Notifications

Email Marketing

Mobile Application

Offline Mode

AI Reports

Multi-Branch Support

Cloud Synchronization

Electronic Signatures

Customer Portal

Supplier Portal

API for Third-Party Integration

Workflow Automation

Business Intelligence Dashboard

---

# Functional Requirement Summary

| Module | Features |
|----------|----------|
| Authentication | Login, Logout, JWT, Password Reset |
| Dashboard | KPIs, Charts, Statistics |
| Customers | CRUD, Statements, History |
| Orders | CRUD, Tracking, Status |
| Deposits | Recording, Allocation |
| Payments | Recording, Allocation, Receipts |
| Balances | Automatic Calculations |
| Reports | PDF, Excel, Printing |
| Users | Roles, Permissions |
| Audit | Complete Activity Logging |
| Notifications | Dashboard, Email, SMS |
| Settings | Company Configuration |
| Search | Global Search |
| Files | Upload, Download |
| Export | PDF, Excel, CSV |

---

# Conclusion

The functional requirements defined in this document represent the minimum feature set required for the initial production release of the BookRecord System.

Every feature implemented in the backend, frontend, and database shall satisfy these functional requirements. Additional modules and capabilities may be introduced in future releases without violating the core business rules established for the system.