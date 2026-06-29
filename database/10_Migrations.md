
---

# 📄 09-migrations.md

```md id="bookrecord-migrations"
# Database Migrations

## Project: BookRecord System  
## Database: PostgreSQL  
## ORM: Entity Framework Core  

---

# 1. Introduction

Migrations are the controlled process of evolving the database schema over time.

They ensure:
- Version control of database structure
- Safe deployment of schema changes
- Consistency across environments
- Traceability of database evolution

---

# 2. Migration Philosophy

The system follows these principles:

- Every schema change must be versioned
- No manual database modifications in production
- Migrations must be reversible where possible
- Migrations must be reviewed before deployment
- Each migration represents a single logical change

---

# 3. EF Core Migration Workflow

The system uses Entity Framework Core for migration management.

Typical workflow:

1. Modify entity models
2. Generate migration
3. Review generated SQL
4. Apply migration to development database
5. Test thoroughly
6. Deploy to staging
7. Deploy to production

---

# 4. Migration Commands

## Create Migration

```bash
dotnet ef migrations add InitialCreate

