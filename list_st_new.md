


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
| 400       | Bad Request.                                   | `Error` object      |
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

#### **Bad Request (400)**
```json
{
  "error": "Issued amount must be positive"
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
### Sample Sequence Diagram

```mermaid
sequenceDiagram
    participant Client as Client
    participant Controller as Controller
    participant Service as Service
    participant Scalardl as Scalardl
    participant Database as Database

    Client ->> Controller: GET /security-tokens<br/>with limit and offset (optional)
    Controller ->> Service: Validate Query Parameters (limit, offset)
    alt Validation Failed
        Service -->> Controller: HTTP 400 Bad Request<br/>Invalid Query Parameters
        Controller -->> Client: HTTP 400 Bad Request<br/>Invalid Query Parameters
    else Validation Succeeded
        Service ->> Scalardl: Validate Data Integrity and Authorization
        Scalardl ->> Database: Validate Data Integrity and Authorization
        alt Scalardl Validation Failed
            Scalardl -->> Service: Validation Error
            Database -->> Scalardl: Validation Error
            Service -->> Controller: HTTP 400 Bad Request<br/>Validation Error
            Controller -->> Client: HTTP 400 Bad Request<br/>Validation Error
        else Scalardl Validation Passed
            Service ->> Database: Fetch STs List
            alt No Security Tokens Found
                Database -->> Service: No Results Found
                Service -->> Controller: HTTP 404 Not Found<br/>No Security Tokens
                Controller -->> Client: HTTP 404 Not Found<br/>No Security Tokens
            else Security Tokens Found
                Database -->> Service: Return List of STs
                Service -->> Controller: HTTP 200 OK<br/>with ST list with pagination
                Controller -->> Client: HTTP 200 OK<br/>with ST list with pagination
            end
        end
    end
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
