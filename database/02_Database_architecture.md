# Database Architecture

**Project:** BookRecord System

**Version:** 1.0

**Database Engine:** PostgreSQL

**ORM:** Entity Framework Core

---

# Table of Contents

1. Introduction
2. Purpose
3. Database Philosophy
4. Architecture Overview
5. Database Design Principles
6. Naming Conventions
7. Schema Organization
8. Data Types
9. Entity Categories
10. Relationships Strategy
11. Data Integrity
12. Security
13. Performance
14. Scalability
15. Backup Strategy
16. Migration Strategy
17. Auditing Strategy
18. Soft Delete Strategy
19. Multi-Tenancy Considerations
20. Future Expansion

---

# 1. Introduction

The BookRecord System stores and manages all operational data using PostgreSQL.

The database is designed to provide:

- High performance
- Data consistency
- Referential integrity
- Security
- Scalability
- Maintainability

The design follows normalization principles while allowing selective denormalization where performance benefits outweigh complexity.

---

# 2. Purpose

The database architecture aims to:

- Store business data efficiently.
- Prevent duplicate information.
- Maintain relationships between entities.
- Ensure reliable financial calculations.
- Support future business growth.
- Enable fast reporting.
- Simplify maintenance.
- Support auditing and compliance.

---

# 3. Database Philosophy

The database follows the following principles:

### Single Source of Truth

Every piece of information is stored once.

Example:

Customer phone numbers are stored only in the Customers table.

Orders reference customers using foreign keys instead of duplicating customer information.

---

### Referential Integrity

Every relationship is enforced using foreign keys.

Example:

Every Order must belong to an existing Customer.

---

### Normalization

The system targets Third Normal Form (3NF).

Goals:

- Eliminate duplicate data.
- Reduce update anomalies.
- Improve consistency.

---

### Auditability

Business transactions must never disappear without trace.

Instead of deleting records permanently:

- Soft Delete is preferred.
- Audit logs record changes.

---

### Scalability

The architecture supports:

- Large datasets
- Multiple concurrent users
- Future modules
- Cloud deployment

---

# 4. Architecture Overview

```
                Next.js Frontend
                        │
                        ▼
            ASP.NET Core Web API
                        │
                        ▼
             Entity Framework Core
                        │
                        ▼
                 PostgreSQL Database
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
 Business Data     Security Data    Audit Data
```

---

# 5. Database Design Principles

The database follows these principles:

- Use surrogate primary keys.
- Use UUIDs where external references are required.
- Every table has a primary key.
- Every relationship uses foreign keys.
- Every table includes audit columns.
- Avoid nullable fields unless necessary.
- Use lookup tables for reusable values.
- Use junction tables for many-to-many relationships.

---

# 6. Naming Conventions

## Tables

Plural nouns.

Examples:

```
Customers
Orders
Payments
Roles
Users
Countries
```

---

## Columns

PascalCase

Examples

```
CustomerId
FirstName
LastName
CreatedAt
UpdatedAt
```

---

## Primary Keys

```
CustomerId
OrderId
PaymentId
```

---

## Foreign Keys

```
CustomerId
OrderId
RoleId
CountryId
```

---

## Junction Tables

```
UserRoles
OrderPayments
RolePermissions
```

---

## Indexes

```
IX_Customers_Email

IX_Orders_Status

IX_Payments_Date
```

---

## Constraints

```
PK_Customers

FK_Orders_Customers

UQ_Customers_Email

CK_Payments_Amount
```

---

# 7. Schema Organization

The database uses logical separation.

## public

Core business data.

Tables include:

- Customers
- Orders
- Payments
- Deposits

---

## security

Authentication and authorization.

Tables include:

- Users
- Roles
- Permissions
- UserRoles

---

## audit

System history.

Tables include:

- AuditLogs
- LoginHistory

---

## configuration

Application settings.

Tables include:

- CompanySettings
- FinancialSettings
- NotificationSettings

---

# 8. Data Types

| Data | PostgreSQL Type |
|-------|-----------------|
| Identifier | UUID |
| Integer | INTEGER |
| Large Integer | BIGINT |
| Decimal | NUMERIC(18,2) |
| Text | TEXT |
| Short Text | VARCHAR |
| Boolean | BOOLEAN |
| Date | DATE |
| DateTime | TIMESTAMP |
| JSON | JSONB |
| Binary | BYTEA |

---

# 9. Entity Categories

## Master Data

Rarely changes.

Examples

- Countries
- Roles
- Permissions
- Settings

---

## Transactional Data

Daily business operations.

Examples

- Orders
- Payments
- Deposits
- Receipts

---

## Reference Data

Lookup information.

Examples

- OrderStatus
- PaymentMethods
- Currencies

---

## Audit Data

System monitoring.

Examples

- AuditLogs
- LoginHistory

---

# 10. Relationship Strategy

The database supports:

### One-to-One

Example

```
User

↓

UserProfile
```

---

### One-to-Many

Example

```
Customer

↓

Orders
```

---

### Many-to-Many

Example

```
Users

↓

UserRoles

↓

Roles
```

---

# 11. Data Integrity

Integrity is enforced using:

- Primary Keys
- Foreign Keys
- Unique Constraints
- Check Constraints
- Transactions
- Cascading Rules

---

# 12. Security

Sensitive data is protected.

Examples

- Passwords stored using hashing.
- JWT refresh tokens stored securely.
- Database access restricted.
- SSL connections enabled.
- Principle of least privilege applied.

---

# 13. Performance

Performance strategies include:

- Indexing frequently searched columns.
- Query optimization.
- Pagination.
- Connection pooling.
- Efficient joins.
- Caching where appropriate.

---

# 14. Scalability

The architecture supports:

- Millions of records.
- Additional business modules.
- Horizontal application scaling.
- Cloud deployment.
- Read replicas (future).

---

# 15. Backup Strategy

Backups include:

- Daily automated backups.
- Weekly full backups.
- Point-in-time recovery (PITR).
- Off-site backup storage.
- Backup verification.

---

# 16. Migration Strategy

Database schema changes are managed using Entity Framework Core Migrations.

Rules:

- Every schema change is version-controlled.
- No manual production changes.
- Rollback scripts are maintained.
- Migrations are reviewed before deployment.

---

# 17. Auditing Strategy

Every critical table includes:

- CreatedAt
- CreatedBy
- UpdatedAt
- UpdatedBy

Important actions are also recorded in the AuditLogs table.

Tracked events include:

- User login
- Customer creation
- Order updates
- Payment recording
- Settings changes

---

# 18. Soft Delete Strategy

Business records are not physically deleted.

Each applicable table contains:

```text
IsDeleted BOOLEAN
DeletedAt TIMESTAMP
DeletedBy UUID
```

Deleted records are excluded from normal queries but remain available for auditing and recovery.

---

# 19. Multi-Tenancy Considerations

Although the initial version targets a single organization, the architecture allows future support for multiple tenants.

A future `TenantId` column can be introduced to business tables to isolate data between organizations without redesigning the schema.

---

# 20. Future Expansion

The database architecture is designed to support additional modules without major restructuring.

Planned future modules include:

- Inventory Management
- Supplier Management
- Purchase Orders
- Accounting
- Expense Tracking
- Warehouse Management
- Customer Portal
- Mobile Application
- Multi-Branch Operations
- Business Intelligence
- AI Insights
- Third-Party Integrations

---

# Database Layer Diagram

```text
+----------------------------------------------------+
|                 PostgreSQL Database                |
+----------------------------------------------------+

           +-------------------------+
           |      Master Data        |
           |-------------------------|
           | Countries               |
           | Roles                   |
           | Permissions             |
           | PaymentMethods          |
           +-------------------------+

           +-------------------------+
           |   Transactional Data    |
           |-------------------------|
           | Customers               |
           | Orders                  |
           | Payments                |
           | Deposits                |
           | Receipts                |
           +-------------------------+

           +-------------------------+
           |     Security Data       |
           |-------------------------|
           | Users                   |
           | UserRoles               |
           | RefreshTokens           |
           +-------------------------+

           +-------------------------+
           |      Audit Data         |
           |-------------------------|
           | AuditLogs               |
           | LoginHistory            |
           +-------------------------+

           +-------------------------+
           | Configuration Data      |
           |-------------------------|
           | CompanySettings         |
           | FinancialSettings       |
           | NotificationSettings    |
           +-------------------------+
```

---

# Summary

The BookRecord System database architecture provides a robust, scalable, and secure foundation for managing business operations. It emphasizes data integrity, maintainability, performance, and future extensibility through well-defined design principles, logical separation of concerns, standardized naming conventions, and enterprise-grade operational practices.

This document establishes the architectural guidelines that all subsequent database design artifacts—including the ER diagram, table specifications, relationships, constraints, indexes, views, and migration strategy—must follow.