# API Design: Create Security Token (ST)

## Overview

This API creates a security token (ST) using ScalarDL. It ensures input validation, interacts with ScalarDL for token creation, and handles potential errors like duplicate token IDs or invalid issuer IDs.

---

### Sequence Diagram

```mermaid
sequenceDiagram
    participant Client as Client
    participant Controller as Controller
    participant Service as Service
    participant ScalardlSDK as ScalarDL SDK Java Client 
    participant Scalardl as ScalarDL

    Client ->> Controller: POST /security-tokens<br/>with issuer_id, token_name, issued_amount, info
    Controller ->> Service: Validate Request Body
    Service ->> Scalardl: call contract (CreateSt)
    alt 
        Scalardl -->> Scalardl: if duplicate entry or invalid issuerId
        Scalardl -->> Service: Error Response
        Service -->> Controller: Invalid issuer_id or Duplicate st_id Found
        Controller -->> Client: Error Response
    else Token Creation Success
        Scalardl -->> Service: Success Response with st_id
        Service -->> Controller: Success Response with st_id
        Controller -->> Client: Success Response 
    end

```

### **Procedure**
1. Initiates a POST request with `issuerId`, `tokenName`, `amount`, and `info`.
2.  Controller forwards the request to Service for processing.
3.  Validates the request and forwards it to the service layer.
4.  Service calls ScalarDL and create the token.
5. If a duplicate st_id or invalid issuerId is detected, an error response is returned.
6. If successful, ScalarDL returns the created `st_id`, and the Client receives a success response.
