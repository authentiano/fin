# Stored Procedures

## Project: BookRecord System  
## Database: PostgreSQL  
## ORM: Entity Framework Core  

---

# 1. Introduction

This document defines all stored procedures used in the BookRecord financial system.

Stored procedures handle **critical financial operations** that require:
- Atomic execution
- High reliability
- Data integrity enforcement
- Reduced application-side complexity
- Centralized business logic in the database layer

---

# 2. Stored Procedure Design Principles

The system follows these rules:

- All financial operations must be atomic
- No partial updates are allowed
- Business-critical logic is enforced at database level
- Procedures must remain simple and predictable
- Audit logging must be supported for sensitive operations
- Procedures must respect schema boundaries (public, security, audit, configuration)

---

# 3. Payment Processing Procedures

## 3.1 Process Payment

### Purpose
Handles full payment lifecycle including:
- Payment creation
- Order allocation
- Order status update
- Financial consistency enforcement

```sql
CREATE OR REPLACE FUNCTION public.ProcessPayment(
    p_customer_id UUID,
    p_order_id UUID,
    p_payment_method_id UUID,
    p_amount NUMERIC(18,2)
)
RETURNS VOID AS
$$
DECLARE
    v_payment_id UUID;
    v_remaining NUMERIC(18,2);
BEGIN

    -- Create Payment
    INSERT INTO Payments (
        PaymentId,
        CustomerId,
        PaymentMethodId,
        Amount,
        PaymentDate
    )
    VALUES (
        gen_random_uuid(),
        p_customer_id,
        p_payment_method_id,
        p_amount,
        NOW()
    )
    RETURNING PaymentId INTO v_payment_id;

    -- Calculate remaining balance
    SELECT (o.TotalAmount - COALESCE(SUM(pa.AmountAllocated),0))
    INTO v_remaining
    FROM Orders o
    LEFT JOIN PaymentAllocations pa ON o.OrderId = pa.OrderId
    WHERE o.OrderId = p_order_id
    GROUP BY o.TotalAmount;

    -- Allocate Payment
    INSERT INTO PaymentAllocations (
        AllocationId,
        PaymentId,
        OrderId,
        AmountAllocated
    )
    VALUES (
        gen_random_uuid(),
        v_payment_id,
        p_order_id,
        LEAST(p_amount, v_remaining)
    );

    -- Update Order Status if fully paid
    IF v_remaining <= p_amount THEN
        UPDATE Orders
        SET StatusId = (
            SELECT StatusId
            FROM OrderStatuses
            WHERE StatusName = 'Paid'
        )
        WHERE OrderId = p_order_id;
    END IF;

END;
$$ LANGUAGE plpgsql;

