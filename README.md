# Task 8 – Stored Procedures and Functions

This task demonstrates how to create a **Stored Procedure** and a **Function** using the Online Bookstore database.

---

## Stored Procedure: `GetUserOrderCount`

**Purpose**: Returns the total number of orders placed by a given user.

```sql
CREATE PROCEDURE GetUserOrderCount(IN input_user_id INT)
BEGIN
    SELECT U.name, COUNT(O.order_id) AS total_orders
    FROM Users U
    LEFT JOIN Orders O ON U.user_id = O.user_id
    WHERE U.user_id = input_user_id
    GROUP BY U.user_id;
END;
```

**Usage:**
```sql
CALL GetUserOrderCount(1);
```

---

## Function: `TotalPaymentByUser`

**Purpose**: Returns the total amount paid by a specific user.

```sql
CREATE FUNCTION TotalPaymentByUser(uid INT)
RETURNS DECIMAL(10,2)
BEGIN
    DECLARE total DECIMAL(10,2);

    SELECT SUM(P.amount)
    INTO total
    FROM Payments P
    JOIN Orders O ON P.order_id = O.order_id
    WHERE O.user_id = uid;

    RETURN IFNULL(total, 0.00);
END;
```

**Usage:**
```sql
SELECT TotalPaymentByUser(1);
```

---

## Output Examples

### Procedure:
```
+--------+--------------+
| name   | total_orders |
+--------+--------------+
| Alice  |      1       |
+--------+--------------+
```

### Function:
```
+-------------+
| total_paid  |
+-------------+
|   499.49     |
+-------------+
```
