# Authentication API

---

## Login

POST

```
/api/auth/login
```

### Request

```json
{
  "username": "admin",
  "password": "password123"
}
```

### Response

```json
{
  "accessToken": "...",
  "refreshToken": "...",
  "expiresIn": 3600
}
```

---

## Logout

POST

```
/api/auth/logout
```

---

## Refresh Token

POST

```
/api/auth/refresh
```

---

## Forgot Password

POST

```
/api/auth/forgot-password
```

---

## Reset Password

POST

```
/api/auth/reset-password
```

---

## Change Password

PUT

```
/api/auth/change-password
```

---

## Get Current User

GET

```
/api/auth/me
```