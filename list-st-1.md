# API Design: List Security Tokens (STs)

## Overview
This API retrieves a list of all security tokens (STs), supporting pagination for efficient data management. 

---

### Sequence Diagram

```mermaid
sequenceDiagram
    participant Client as Client
    participant Controller as Controller
    participant Service as Service
    participant Database as Database

    Client ->> Controller: GET /security-tokens<br/>with limit and offset (optional)
    Controller ->> Service: Retrieve Security tokens (limit, offset)
    Service ->> Database: Scan Security tokens 
    Database -->> Service: Return List of Security tokens
    Service -->> Controller: Return List of Security tokens with pagination
    Controller -->> Client: Success responce
    end

```

---

## **Procedure**

1.  Initiates a `GET` request to `/security-tokens` with optional query parameters `limit` and `offset`.
2.  Controller forwards the request to Service for processing.
3.  Service queries the Database to retrieve the list of security tokens, applying pagination if provided.
3.  Executes the query to retrieve the ST list. 
4.  Database returns the list of tokens to Service.
5.  Service formats the response with tokens and pagination details.
6.  Controller returns a success response with the token list.
---

