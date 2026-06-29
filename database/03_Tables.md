# Database Tables (Data Dictionary)

**Project:** BookRecord System

**Database:** PostgreSQL

**Version:** 1.0

---

# Table of Contents

1. Introduction
2. Table Categories
3. Audit Columns
4. Business Tables
5. Security Tables
6. Configuration Tables
7. Audit Tables
8. Lookup Tables
9. Table Summary

---

# 1. Introduction

This document defines every table used by the BookRecord System.

For each table, the following information is provided:

- Purpose
- Primary Key
- Foreign Keys
- Columns
- Data Types
- Nullability
- Default Values
- Business Notes

The document serves as the authoritative database dictionary for PostgreSQL and Entity Framework Core.

---

# 2. Table Categories

The database is divided into the following logical categories:

## Business Tables

- Customers
- Orders
- Suppliers
- Payments
- Deposits
- PaymentAllocations
- Receipts

---

## Security Tables

- Users
- Roles
- Permissions
- UserRoles
- RolePermissions
- RefreshTokens

---

## Configuration Tables

- CompanySettings
- FinancialSettings
- NotificationSettings

---

## Audit Tables

- AuditLogs
- LoginHistory

---

## Lookup Tables

- Countries
- PaymentMethods
- OrderStatuses

---

# 3. Standard Audit Columns

Every transactional table should include the following columns.

| Column | Type | Description |
|----------|------|-------------|
| CreatedAt | TIMESTAMP | Creation timestamp |
| CreatedBy | UUID | User who created record |
| UpdatedAt | TIMESTAMP | Last update |
| UpdatedBy | UUID | User who updated |
| IsDeleted | BOOLEAN | Soft delete flag |
| DeletedAt | TIMESTAMP | Deletion date |
| DeletedBy | UUID | User who deleted |

---

# 4. Business Tables

---

# Customers

## Purpose

Stores customer information.

---

## Primary Key

CustomerId

---

## Columns

| Column | Type | Required | Description |
|---------|------|----------|-------------|
| CustomerId | UUID | Yes | Primary Key |
| CustomerNumber | VARCHAR(30) | Yes | Business identifier |
| FirstName | VARCHAR(100) | Yes | First name |
| LastName | VARCHAR(100) | Yes | Last name |
| Phone | VARCHAR(30) | Yes | Contact number |
| Email | VARCHAR(255) | No | Email address |
| CountryId | UUID | Yes | Customer country |
| Address | TEXT | No | Physical address |
| Status | VARCHAR(20) | Yes | Active / Inactive |
| CreatedAt | TIMESTAMP | Yes | Audit |
| UpdatedAt | TIMESTAMP | No | Audit |
| IsDeleted | BOOLEAN | Yes | Soft Delete |

---

## Relationships

Customer

↓

Orders

Customer

↓

Payments

Customer

↓

Deposits

---

# Suppliers

## Purpose

Stores supplier information.

---

## Columns

| Column | Type |
|---------|------|
| SupplierId | UUID |
| SupplierName | VARCHAR(200) |
| ContactPerson | VARCHAR(200) |
| Phone | VARCHAR(30) |
| Email | VARCHAR(255) |
| Address | TEXT |
| Status | VARCHAR(20) |

---

# Orders

## Purpose

Stores customer orders.

---

## Columns

| Column | Type |
|---------|------|
| OrderId | UUID |
| OrderNumber | VARCHAR(50) |
| CustomerId | UUID |
| SupplierId | UUID |
| OrderDate | DATE |
| ExpectedArrival | DATE |
| TotalAmount | NUMERIC(18,2) |
| StatusId | UUID |
| Notes | TEXT |

---

## Relationships

Many Orders belong to one Customer.

Many Orders belong to one Supplier.

Many Orders receive many Payments through PaymentAllocations.

---

# Payments

## Purpose

Stores all customer payments.

---

## Columns

| Column | Type |
|---------|------|
| PaymentId | UUID |
| PaymentReference | VARCHAR(50) |
| CustomerId | UUID |
| PaymentMethodId | UUID |
| PaymentDate | DATE |
| Amount | NUMERIC(18,2) |
| Currency | VARCHAR(10) |
| Notes | TEXT |

---

# Deposits

## Purpose

Stores advance deposits.

---

## Columns

| Column | Type |
|---------|------|
| DepositId | UUID |
| CustomerId | UUID |
| DepositDate | DATE |
| Amount | NUMERIC(18,2) |
| Balance | NUMERIC(18,2) |
| Notes | TEXT |

---

# PaymentAllocations

## Purpose

Resolves the many-to-many relationship between Orders and Payments.

---

## Columns

| Column | Type |
|---------|------|
| AllocationId | UUID |
| PaymentId | UUID |
| OrderId | UUID |
| AllocatedAmount | NUMERIC(18,2) |
| AllocationDate | DATE |

---

# Receipts

## Purpose

Stores generated receipts.

---

## Columns

| Column | Type |
|---------|------|
| ReceiptId | UUID |
| ReceiptNumber | VARCHAR(50) |
| PaymentId | UUID |
| GeneratedDate | TIMESTAMP |
| PDFPath | TEXT |

---

# 5. Security Tables

---

# Users

## Purpose

Stores application users.

---

## Columns

| Column | Type |
|---------|------|
| UserId | UUID |
| Username | VARCHAR(100) |
| Email | VARCHAR(255) |
| PasswordHash | TEXT |
| IsActive | BOOLEAN |
| LastLogin | TIMESTAMP |

---

# Roles

| Column | Type |
|---------|------|
| RoleId | UUID |
| Name | VARCHAR(100) |
| Description | TEXT |

---

# Permissions

| Column | Type |
|---------|------|
| PermissionId | UUID |
| Name | VARCHAR(100) |
| Description | TEXT |

---

# UserRoles

Many-to-many relationship.

| Column | Type |
|---------|------|
| UserRoleId | UUID |
| UserId | UUID |
| RoleId | UUID |

---

# RolePermissions

Many-to-many relationship.

| Column | Type |
|---------|------|
| RolePermissionId | UUID |
| RoleId | UUID |
| PermissionId | UUID |

---

# RefreshTokens

| Column | Type |
|---------|------|
| TokenId | UUID |
| UserId | UUID |
| Token | TEXT |
| ExpiryDate | TIMESTAMP |
| Revoked | BOOLEAN |

---

# 6. Configuration Tables

---

# CompanySettings

| Column | Type |
|---------|------|
| CompanyId | UUID |
| CompanyName | VARCHAR(255) |
| Phone | VARCHAR(50) |
| Email | VARCHAR(255) |
| Address | TEXT |
| LogoPath | TEXT |
| Currency | VARCHAR(10) |

---

# FinancialSettings

| Column | Type |
|---------|------|
| FinancialSettingsId | UUID |
| TaxRate | NUMERIC(5,2) |
| InvoicePrefix | VARCHAR(20) |
| ReceiptPrefix | VARCHAR(20) |
| DecimalPlaces | INTEGER |

---

# NotificationSettings

| Column | Type |
|---------|------|
| NotificationSettingsId | UUID |
| EmailEnabled | BOOLEAN |
| SmsEnabled | BOOLEAN |
| PaymentReminder | BOOLEAN |

---

# 7. Audit Tables

---

# AuditLogs

## Purpose

Stores all system activities.

---

## Columns

| Column | Type |
|---------|------|
| AuditId | UUID |
| UserId | UUID |
| Action | VARCHAR(100) |
| Module | VARCHAR(100) |
| EntityId | UUID |
| OldValue | JSONB |
| NewValue | JSONB |
| CreatedAt | TIMESTAMP |

---

# LoginHistory

| Column | Type |
|---------|------|
| LoginId | UUID |
| UserId | UUID |
| LoginTime | TIMESTAMP |
| IPAddress | VARCHAR(50) |
| Browser | TEXT |
| Success | BOOLEAN |

---

# 8. Lookup Tables

---

# Countries

| Column | Type |
|---------|------|
| CountryId | UUID |
| CountryName | VARCHAR(100) |
| CountryCode | VARCHAR(5) |

---

# PaymentMethods

| Column | Type |
|---------|------|
| PaymentMethodId | UUID |
| Name | VARCHAR(50) |

Examples:

- Cash
- Mobile Money
- Bank Transfer
- Credit Card

---

# OrderStatuses

| Column | Type |
|---------|------|
| StatusId | UUID |
| StatusName | VARCHAR(50) |

Examples:

- Pending
- Processing
- Shipped
- Arrived
- Completed
- Cancelled

---

# 9. Table Summary

| Category | Tables |
|----------|--------|
| Business | Customers, Orders, Suppliers, Payments, Deposits, PaymentAllocations, Receipts |
| Security | Users, Roles, Permissions, UserRoles, RolePermissions, RefreshTokens |
| Configuration | CompanySettings, FinancialSettings, NotificationSettings |
| Audit | AuditLogs, LoginHistory |
| Lookup | Countries, PaymentMethods, OrderStatuses |

---

# Database Naming Standards

## Primary Keys

```
CustomerId
OrderId
PaymentId
RoleId
```

---

## Foreign Keys

```
CustomerId

OrderId

RoleId

UserId

CountryId
```

---

## Timestamp Columns

```
CreatedAt

UpdatedAt

DeletedAt
```

---

## Soft Delete Columns

```
IsDeleted

DeletedBy

DeletedAt
```

---

# Entity Framework Core Mapping Notes

- Every table maps to a single Entity class.
- UUID columns map to `Guid`.
- `NUMERIC(18,2)` maps to `decimal`.
- `TIMESTAMP` maps to `DateTime`.
- `JSONB` maps to `JsonDocument` or strongly typed value objects where appropriate.
- Many-to-many relationships should use explicit junction entities (`UserRole`, `RolePermission`, `PaymentAllocation`) rather than implicit EF Core many-to-many mappings to allow auditing and future expansion.

---

# Future Tables (Planned)

The following tables are reserved for future versions of the system:

## Inventory

- Products
- Categories
- ProductCategories
- Warehouses
- Stock
- StockMovements

## Procurement

- PurchaseOrders
- PurchaseOrderItems
- Vendors

## Accounting

- Accounts
- JournalEntries
- LedgerEntries

## CRM

- Leads
- Opportunities
- Activities

## Notifications

- Notifications
- EmailTemplates
- SmsTemplates

## File Management

- Attachments
- Documents

---

# Summary

This document defines the core database tables for Version 1 of the BookRecord System. It establishes a consistent data dictionary, standard naming conventions, and table structures that support PostgreSQL, Entity Framework Core, and the application's business requirements. Future modules should follow the same design principles to maintain consistency, scalability, and data integrity.