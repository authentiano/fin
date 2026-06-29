# Seed Data

## Project: BookRecord System  
## Database: PostgreSQL  
## ORM: Entity Framework Core  

---

# 1. Introduction

Seed data represents the **initial dataset required to bootstrap the BookRecord System**.

It ensures the system is functional immediately after deployment by providing:
- Default roles
- Permissions
- Order statuses
- Payment methods
- Configuration settings
- Reference data

---

# 2. Seed Data Philosophy

The system follows these principles:

- Seed data must be minimal but complete
- Only reference and configuration data is seeded
- No transactional data is preloaded
- Seed data must be idempotent (safe to re-run)
- Seed data must support system security from day one

---

# 3. Core Reference Data

## 3.1 Countries

```sql
INSERT INTO Countries (CountryId, CountryCode, CountryName)
VALUES
(gen_random_uuid(), 'UG', 'Uganda'),
(gen_random_uuid(), 'KE', 'Kenya'),
(gen_random_uuid(), 'TZ', 'Tanzania');