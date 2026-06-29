# Entity Relationship Diagram (ERD)

**Project:** BookRecord System

**Version:** 1.0

**Database:** PostgreSQL

---

# Table of Contents

1. Introduction
2. Purpose
3. Conceptual Data Model
4. Logical Data Model
5. Physical Data Model
6. Core Business Entities
7. Entity Relationships
8. Relationship Cardinality
9. Business Rules
10. Entity Dependency Graph
11. ER Diagram
12. Future Expansion

---

# 1. Introduction

The Entity Relationship Diagram (ERD) provides a visual and logical representation of the database structure for the BookRecord System.

It identifies:

- Business entities
- Relationships
- Cardinality
- Ownership
- Dependencies

The ERD acts as the foundation for database implementation using PostgreSQL and Entity Framework Core.

---

# 2. Purpose

The objectives of the ERD are:

- Identify all business entities.
- Define relationships between entities.
- Prevent duplicate data.
- Support normalization.
- Guide database implementation.
- Support Entity Framework Core mapping.
- Improve maintainability.

---

# 3. Conceptual Data Model

The system consists of five major domains.

```
Authentication

↓

Business

↓

Finance

↓

Reporting

↓

Configuration
```

---

## Authentication Domain

- Users
- Roles
- Permissions
- UserRoles
- RefreshTokens

---

## Business Domain

- Customers
- Orders
- Suppliers
- Countries

---

## Finance Domain

- Payments
- Deposits
- Receipts
- PaymentAllocations

---

## Reporting Domain

- AuditLogs
- LoginHistory

---

## Configuration Domain

- CompanySettings
- FinancialSettings
- NotificationSettings

---

# 4. Logical Data Model

The logical model defines how business entities relate.

```
Customer

↓

Orders

↓

Payments

↓

Payment Allocation

↓

Receipt
```

Another example

```
User

↓

UserRoles

↓

Roles

↓

RolePermissions

↓

Permissions
```

---

# 5. Physical Data Model

The physical implementation uses PostgreSQL tables.

| Entity | Table |
|---------|-------|
| Customer | Customers |
| Order | Orders |
| Payment | Payments |
| Deposit | Deposits |
| User | Users |
| Role | Roles |
| Permission | Permissions |
| Country | Countries |

---

# 6. Core Business Entities

## Customers

Stores customer information.

Relationships

- One customer has many orders.
- One customer has many payments.
- One customer has many deposits.

---

## Orders

Stores customer orders.

Relationships

- Belongs to one customer.
- Belongs to one supplier.
- Has many payments.
- Has one status.

---

## Payments

Stores financial payments.

Relationships

- Belongs to one customer.
- May pay multiple orders.

---

## Deposits

Stores advance customer deposits.

Relationships

- Belongs to one customer.

---

## Suppliers

Represents shipping or business suppliers.

Relationships

- One supplier has many orders.

---

## Countries

Reference table.

Relationships

- One country has many customers.

---

## Users

Application users.

Relationships

- One user has many audit logs.
- One user may have multiple roles.

---

## Roles

Security roles.

Examples

- Administrator
- Manager
- Staff
- Viewer

---

## Permissions

System permissions.

Examples

- Create Customer
- Delete Customer
- View Reports
- Configure Settings

---

## AuditLogs

Stores all important actions.

---

## CompanySettings

Stores company configuration.

---

# 7. Entity Relationships

## Customer → Orders

```
Customer

1

↓

∞

Orders
```

---

## Customer → Payments

```
Customer

1

↓

∞

Payments
```

---

## Customer → Deposits

```
Customer

1

↓

∞

Deposits
```

---

## Supplier → Orders

```
Supplier

1

↓

∞

Orders
```

---

## Country → Customers

```
Country

1

↓

∞

Customers
```

---

## Order → Payment Allocation

```
Order

1

↓

∞

PaymentAllocation
```

---

## Payment → Payment Allocation

```
Payment

1

↓

∞

PaymentAllocation
```

---

## User → UserRoles

```
User

1

↓

∞

UserRoles
```

---

## Role → UserRoles

```
Role

1

↓

∞

UserRoles
```

---

## Role → Permissions

```
Role

1

↓

∞

RolePermissions

∞

↓

1

Permission
```

---

## User → AuditLogs

```
User

1

↓

∞

AuditLogs
```

---

# 8. Relationship Cardinality

| Parent | Child | Cardinality |
|---------|-------|-------------|
| Country | Customers | 1:N |
| Customer | Orders | 1:N |
| Customer | Payments | 1:N |
| Customer | Deposits | 1:N |
| Supplier | Orders | 1:N |
| Order | PaymentAllocations | 1:N |
| Payment | PaymentAllocations | 1:N |
| User | UserRoles | 1:N |
| Role | UserRoles | 1:N |
| Role | RolePermissions | 1:N |
| Permission | RolePermissions | 1:N |
| User | AuditLogs | 1:N |

---

# 9. Business Rules

## Customer

A customer may exist without an order.

---

## Orders

Every order must belong to exactly one customer.

---

## Payments

Every payment belongs to one customer.

A payment may settle multiple orders.

---

## Payment Allocation

A payment allocation connects one payment to one order.

This resolves the many-to-many relationship between Payments and Orders.

---

## Deposits

Deposits reduce outstanding balances.

---

## Suppliers

A supplier may exist before receiving orders.

---

## Roles

Users may have multiple roles.

---

## Permissions

Permissions are assigned through roles.

---

## Audit Logs

Every audit record belongs to one user.

---

# 10. Entity Dependency Graph

```
Countries
     │
     ▼
Customers
     │
     ├─────────────┐
     ▼             ▼
Orders         Payments
     │             │
     └──────┐      │
            ▼      ▼
      PaymentAllocations
            │
            ▼
         Receipts


Users
 │
 ▼
UserRoles
 │
 ▼
Roles
 │
 ▼
RolePermissions
 │
 ▼
Permissions

Users
 │
 ▼
AuditLogs

Settings
 │
 ├── CompanySettings
 ├── FinancialSettings
 └── NotificationSettings
```

---

# 11. Complete ER Diagram (Text Representation)

```
+-------------+
| Countries   |
+-------------+
        |
        | 1
        |
        | N
+----------------+
| Customers      |
+----------------+
        |
        | 1
        |
        | N
+----------------+
| Orders         |
+----------------+
        |
        | 1
        |
        | N
+------------------------+
| PaymentAllocations     |
+------------------------+
        ^
        |
        | N
        |
        | 1
+----------------+
| Payments       |
+----------------+
        ^
        |
        | N
        |
        | 1
+----------------+
| Customers      |
+----------------+

Customers
     |
     | 1
     |
     | N
+----------------+
| Deposits       |
+----------------+

Suppliers
     |
     | 1
     |
     | N
Orders

Users
 |
 | 1
 |
 | N
UserRoles
 |
 | N
 |
 | 1
Roles
 |
 | 1
 |
 | N
RolePermissions
 |
 | N
 |
 | 1
Permissions

Users
 |
 | 1
 |
 | N
AuditLogs
```

---

# 12. Future Expansion

The ERD has been intentionally designed to support future modules without major schema redesign.

Planned entities include:

## Inventory

- Products
- Categories
- Stock
- Warehouses
- StockMovements

---

## Procurement

- PurchaseOrders
- PurchaseOrderItems
- Vendors

---

## Accounting

- Accounts
- Journals
- Transactions
- Ledgers

---

## CRM

- Leads
- Opportunities
- Activities

---

## Logistics

- Shipments
- Tracking
- DeliveryNotes

---

## Human Resources

- Employees
- Departments
- Attendance
- Payroll

---

# ERD Design Principles

The ER diagram follows these architectural principles:

- Third Normal Form (3NF)
- Surrogate Primary Keys
- Foreign Key Constraints
- Soft Delete Support
- Audit Columns
- Lookup Tables
- Junction Tables for Many-to-Many Relationships
- Separation of Authentication, Business, Finance, and Configuration Domains
- Scalability for Future Modules

---

# Summary

The Entity Relationship Diagram defines the structural blueprint of the BookRecord System database. It models the core business entities, their relationships, and the rules governing their interactions. This document serves as the authoritative reference for implementing the PostgreSQL schema, configuring Entity Framework Core entities, and ensuring consistency across the backend, frontend, and reporting layers.