


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
| 404       | No security tokens found.                      | `Error` object      |
| 500       | Internal server error.                         | `Error` object      |


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

---

## **Procedure**

1.  Sends a `GET` request to `/security-tokens` with optional query parameters `limit` and `offset`.
2.  Handles the request:
   - Validates the query parameters:
     - Ensures `limit` is a positive integer.
     - Ensures `offset` is a non-negative integer.
   - Fetches the list of STs from the database, applying pagination based on limit and offset.
3.  Executes the query to retrieve the ST list. 
4.  Returns the retrieved list or propagates errors:
   - Sends the array of ST objects.
   - Sends appropriate status codes and error messages.

---
