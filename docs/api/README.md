# BookRecord System API Documentation

## Overview

This directory contains the API specification for the BookRecord System.

The API follows RESTful principles and uses JSON for request and response bodies.

---

## Base URL

Development

```
http://localhost:5000/api
```

Production

```
https://api.bookrecord.com/api
```

---

## Authentication

All protected endpoints require a JWT Bearer Token.

Example Header

```
Authorization: Bearer <access_token>
```

---

## Standard Response Format

### Success

```json
{
  "success": true,
  "message": "Operation completed successfully.",
  "data": {}
}
```

---

### Error

```json
{
  "success": false,
  "message": "Validation failed.",
  "errors": [
    {
      "field": "email",
      "message": "Email is required."
    }
  ]
}
```

---

## HTTP Status Codes

| Code | Meaning |
|------|---------|
|200|Success|
|201|Created|
|204|No Content|
|400|Bad Request|
|401|Unauthorized|
|403|Forbidden|
|404|Not Found|
|409|Conflict|
|422|Validation Error|
|500|Internal Server Error|

---

## API Version

Version 1

```
/api/v1/
```

---

## Naming Conventions

Resources use plural nouns.

Examples

```
/customers

/orders

/payments

/users
```

---

## Pagination

```
GET /customers?page=1&pageSize=20
```

---

## Filtering

```
GET /orders?status=Pending
```

---

## Searching

```
GET /customers?search=John
```

---

## Sorting

```
GET /customers?sort=name
```

---

## Date Filtering

```
GET /payments?from=2026-01-01&to=2026-01-31
```

---

## Modules

- Authentication
- Dashboard
- Customers
- Orders
- Payments
- Reports
- Settings
