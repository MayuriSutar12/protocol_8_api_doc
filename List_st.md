

# API Design: List Security Tokens (STs)

## Overview
This API retrieves a list of all security tokens (STs), supporting pagination for efficient data management. It validates query parameters and handles errors gracefully to ensure a reliable experience.

---

## **API Endpoint**
`GET /security-tokens`

## **Summary**
Fetches a list of all security tokens (STs) with optional pagination using query parameters `limit` and `offset`.

---

## **Query Parameters**

| Name    | Type     | Description                                                | Default Value |
|---------|----------|------------------------------------------------------------|---------------|
| limit   | Integer  | Number of items to retrieve.                               | 10            |
| offset  | Integer  | Number of items to skip before starting to collect results.| 0             |

---

## **Responses**

| HTTP Code | Description                                    | Response Body       |
|-----------|------------------------------------------------|---------------------|
| 200       | Successfully retrieved list of security tokens.| Array of `ST` objects|
| 400       | Malformed request, validation failed.          | `Error` object      |
| 404       | Resource not found.                           | `Error` object      |
| 500       | Internal server error.                        | `Error` object      |
| 503       | Service unavailable (e.g., under maintenance).| `Error` object      |

---

### **Example Success Response (200)**

```json
[
  {
    "st_id": "issuer123-tokenABC",
    "state": "CREATED",
    "total_issued_amount": 1000,
    "total_published_amount": 0,
    "info": {
      "info_prop_1": "propertyValue"
    }
  }
]
```

### **Example Error Responses**

#### **Malformed Request (400)**
```json
{
  "error": "Invalid 'limit' parameter. Must be a positive integer."
}
```

#### **Resource Not Found (404)**
```json
{
  "error": "No security tokens found."
}
```

#### **Internal Server Error (500)**
```json
{
  "error": "An unexpected error occurred."
}
```

#### **Service Unavailable (503)**
```json
{
  "error": "Service is temporarily unavailable. Please try again later."
}
```

---

## **Procedure**

1. **Frontend:** Sends a `GET` request to `/security-tokens` with optional query parameters `limit` and `offset`.
2. **Backend:** Handles the request:
   - Validates the query parameters:
     - Ensures `limit` is a positive integer.
     - Ensures `offset` is a non-negative integer.
   - Fetches the list of STs from the database, applying pagination.
3. **Database:** Executes the query to retrieve the ST list based on `limit` and `offset`.
4. **Backend Response:** Returns the retrieved list or propagates errors:
   - **Success (200):** Sends the array of ST objects.
   - **Error:** Sends appropriate status codes and error messages.

---
