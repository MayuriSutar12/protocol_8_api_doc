# API Design: Create Security Token (ST)

## Overview
This API facilitates the creation of a new security token (ST) for off-chain transactions. It validates user input, integrates with a scalardl for state management, and ensures error handling to maintain system integrity.

---

### Sequence Diagram

```mermaid
sequenceDiagram
    participant Client as Client
    participant Controller as Controller
    participant Service as Service
    participant Scalardl as ScalarDL
    participant Database as Database

    Client ->> Controller: POST /security-tokens<br/>with issuer_id, token_name, issued_amount, info
    Controller ->> Service: Validate Request Body
    alt Validation Failed
        Service -->> Controller: Validation Error
        Controller -->> Client: HTTP 400 Bad Request<br/>Error Response
    else Validation Succeeded
        Service ->> Scalardl: Validate issuer_id and Check for Duplicate st_id
        Scalardl ->> Database: Validate issuer_id and Check for Duplicate st_id
        alt Invalid issuer_id or Duplicate st_id Found
            Scalardl -->> Service: Validation Error
            Database -->> Scalardl: Validation Error
            Service -->> Controller: Error Response
            Controller -->> Client: HTTP 400/409 Error Response
        else Validation Passed
            Service ->> Database: Create Security Token Entry<br/>Update Issuer’s Account Balance
            Database -->> Service: Token Creation Confirmed
            Service -->> Controller: Success Response with st_id
            Controller -->> Client: HTTP 201 Created<br/>with St_id
        end
    end
```

### **Procedure**
1. Initiates a POST request with `issuerId`, `tokenName`, `amount`, and `info`.
2.  Validates the request and forwards it to the service layer.
3. - Validates data against scalardl state.
   - Ensures no conflicts (e.g., duplicate `st_id`, invalid `issuerId`).
4. Confirms the validity of `issuerId` and checks for existing `st_id` conflicts.
5. Creates the token entry and updates the issuer’s account balances.
6. Returns success response with `st_id` or propagates errors to the controller.
7. Sends appropriate HTTP response to the client.

---

