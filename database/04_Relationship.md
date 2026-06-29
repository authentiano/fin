# Database Relationships

**Project:** BookRecord System

**Database:** PostgreSQL

**ORM:** Entity Framework Core

**Version:** 1.0

---

# Table of Contents

1. Introduction
2. Relationship Types
3. Relationship Principles
4. Business Relationships
5. Security Relationships
6. Configuration Relationships
7. Audit Relationships
8. Entity Framework Navigation Properties
9. Cascade Rules
10. Referential Integrity Rules
11. Relationship Summary

---

# 1. Introduction

This document defines how entities within the BookRecord System relate to one another.

The objectives are to:

- Maintain referential integrity.
- Prevent orphaned records.
- Define ownership.
- Support Entity Framework Core navigation properties.
- Ensure business rules are enforced.

---

# 2. Relationship Types

The system uses the following relationship types.

## One-to-One (1:1)

One parent owns exactly one child.

Example

```
User
    │
    ▼
UserProfile
```

---

## One-to-Many (1:N)

One parent owns multiple children.

Example

```
Customer
    │
    ▼
Orders
```

---

## Many-to-Many (N:M)

Implemented using a junction table.

Example

```
Users
   │
UserRoles
   │
Roles
```

---

# 3. Relationship Principles

The following principles apply throughout the database.

- Every child record references a valid parent.
- Parent records should never reference children directly using foreign keys.
- Many-to-many relationships must use junction tables.
- Business transactions are never physically deleted.
- Foreign keys enforce consistency.
- Nullable foreign keys are only used when business rules allow optional relationships.

---

# 4. Business Relationships

---

## Country → Customers

Relationship

```
Country (1)
      │
      ▼
Customers (N)
```

### Cardinality

One Country

↓

Many Customers

### Foreign Key

```
Customers.CountryId
```

### Business Rule

Every customer belongs to one country.

A country may contain many customers.

Deleting a country is prohibited while customers exist.

---

## Customer → Orders

Relationship

```
Customer (1)

      │

      ▼

Orders (N)
```

### Foreign Key

```
Orders.CustomerId
```

### Business Rules

A customer may have zero or many orders.

Every order must belong to one customer.

An order cannot exist without a customer.

Customer deletion is prevented if orders exist.

---

## Supplier → Orders

Relationship

```
Supplier (1)

      │

      ▼

Orders (N)
```

### Foreign Key

```
Orders.SupplierId
```

Business Rule

Every order is assigned to one supplier.

A supplier may process many orders.

---

## Customer → Payments

Relationship

```
Customer (1)

      │

      ▼

Payments (N)
```

### Foreign Key

```
Payments.CustomerId
```

Business Rules

A customer may make many payments.

Every payment belongs to one customer.

---

## Customer → Deposits

Relationship

```
Customer (1)

      │

      ▼

Deposits (N)
```

### Foreign Key

```
Deposits.CustomerId
```

Business Rules

Customers may hold multiple deposits.

Deposits reduce outstanding balances.

---

## Orders ↔ Payments

Relationship

Many-to-Many

Implemented by

```
PaymentAllocations
```

Diagram

```
Orders

      │

      ▼

PaymentAllocations

      ▲

      │

Payments
```

Business Rule

One payment may settle multiple orders.

One order may receive multiple payments.

---

## Payment → Receipt

Relationship

```
Payment (1)

      │

      ▼

Receipt (1)
```

Business Rule

Every completed payment generates one receipt.

Receipts cannot exist without payments.

---

# 5. Security Relationships

---

## Users ↔ Roles

Relationship

Many-to-Many

Implemented using

```
UserRoles
```

Diagram

```
Users

   │

UserRoles

   │

Roles
```

Business Rule

One user may have multiple roles.

One role may belong to many users.

---

## Roles ↔ Permissions

Relationship

Many-to-Many

Implemented using

```
RolePermissions
```

Diagram

```
Roles

   │

RolePermissions

   │

Permissions
```

Business Rule

Permissions are granted through roles.

Users inherit permissions from assigned roles.

---

## Users → RefreshTokens

Relationship

```
Users (1)

      │

      ▼

RefreshTokens (N)
```

Business Rule

Users may have multiple active refresh tokens.

Expired tokens are retained for auditing until cleanup.

---

# 6. Configuration Relationships

Configuration tables are independent.

Examples

```
CompanySettings

FinancialSettings

NotificationSettings
```

Business Rule

Only one active configuration record exists per settings category.

---

# 7. Audit Relationships

---

## Users → AuditLogs

Relationship

```
Users (1)

      │

      ▼

AuditLogs (N)
```

Business Rule

Every audit log belongs to one user.

Audit logs are never deleted.

---

## Users → LoginHistory

Relationship

```
Users (1)

      │

      ▼

LoginHistory (N)
```

Business Rule

Every login attempt is recorded.

Successful and failed logins are retained.

---

# 8. Entity Framework Core Navigation Properties

## Customer

```csharp
public ICollection<Order> Orders { get; set; }

public ICollection<Payment> Payments { get; set; }

public ICollection<Deposit> Deposits { get; set; }
```

---

## Order

```csharp
public Customer Customer { get; set; }

public Supplier Supplier { get; set; }

public ICollection<PaymentAllocation> PaymentAllocations { get; set; }
```

---

## Payment

```csharp
public Customer Customer { get; set; }

public ICollection<PaymentAllocation> PaymentAllocations { get; set; }

public Receipt Receipt { get; set; }
```

---

## User

```csharp
public ICollection<UserRole> UserRoles { get; set; }

public ICollection<AuditLog> AuditLogs { get; set; }

public ICollection<RefreshToken> RefreshTokens { get; set; }
```

---

## Role

```csharp
public ICollection<UserRole> UserRoles { get; set; }

public ICollection<RolePermission> RolePermissions { get; set; }
```

---

# 9. Cascade Rules

The following rules apply when parent records are modified.

| Parent | Child | On Delete | On Update |
|----------|---------|------------|------------|
| Country | Customers | Restrict | Cascade |
| Customer | Orders | Restrict | Cascade |
| Customer | Payments | Restrict | Cascade |
| Customer | Deposits | Restrict | Cascade |
| Supplier | Orders | Restrict | Cascade |
| Payment | Receipt | Cascade | Cascade |
| Payment | PaymentAllocation | Cascade | Cascade |
| Order | PaymentAllocation | Cascade | Cascade |
| User | UserRoles | Cascade | Cascade |
| Role | UserRoles | Restrict | Cascade |
| User | AuditLogs | Restrict | Cascade |

---

# 10. Referential Integrity Rules

The following rules guarantee consistency.

## Rule 1

Every Order must reference an existing Customer.

---

## Rule 2

Every Payment must reference an existing Customer.

---

## Rule 3

Every Order must reference an existing Supplier.

---

## Rule 4

PaymentAllocation must reference:

- Existing Payment
- Existing Order

---

## Rule 5

Every UserRole references:

- Existing User
- Existing Role

---

## Rule 6

Every RolePermission references:

- Existing Role
- Existing Permission

---

## Rule 7

Audit logs cannot reference deleted users.

---

## Rule 8

Receipt cannot exist without Payment.

---

## Rule 9

A Customer cannot be deleted while related Orders, Payments, or Deposits exist.

---

## Rule 10

Soft-deleted records remain available for reporting and auditing but are excluded from standard application queries.

---

# 11. Relationship Summary

## Business Domain

| Parent | Child | Type |
|----------|---------|------|
| Country | Customers | 1:N |
| Customer | Orders | 1:N |
| Customer | Payments | 1:N |
| Customer | Deposits | 1:N |
| Supplier | Orders | 1:N |
| Payment | Receipt | 1:1 |
| Order | PaymentAllocation | 1:N |
| Payment | PaymentAllocation | 1:N |

---

## Security Domain

| Parent | Child | Type |
|----------|---------|------|
| User | UserRoles | 1:N |
| Role | UserRoles | 1:N |
| Role | RolePermissions | 1:N |
| Permission | RolePermissions | 1:N |
| User | RefreshTokens | 1:N |

---

## Audit Domain

| Parent | Child | Type |
|----------|---------|------|
| User | AuditLogs | 1:N |
| User | LoginHistory | 1:N |

---

# Relationship Design Principles

The BookRecord System follows these architectural principles:

- Strong referential integrity through foreign keys.
- Explicit junction tables for many-to-many relationships.
- Restrictive deletes for transactional data.
- Cascading updates where appropriate.
- Soft deletes instead of physical deletion.
- Navigation properties optimized for Entity Framework Core.
- Business rules enforced at both the database and application layers.

---

# Summary

The relationships defined in this document establish the structural integrity of the BookRecord System database. By clearly specifying ownership, cardinality, cascade behavior, and referential integrity, the design supports a scalable, maintainable, and secure PostgreSQL implementation. These relationships will directly inform Entity Framework Core configurations, repository implementations, and application business logic.