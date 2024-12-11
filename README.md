# API Design: Create Security Token (ST)

## Overview
This API facilitates the creation of a new security token (ST) for off-chain transactions. It validates user input, integrates with a ledger for state management, and ensures error handling to maintain system integrity.

---

## ****
`POST /api/st/create`

## **Summary**
Creates a new security token (ST) and associates it with the issuer.

---

## **Request Body**

| Name             | Description                              | Schema               |
|------------------|------------------------------------------|----------------------|
| CreateStRequest  | Details for creating the security token | `CreateStRequest`   |

### **Example Request Body**

```json
{
  "issuerId": "issuer123",
  "tokenName": "MyToken",
  "amount": 1000,
  "info": {
    "description": "Sample token",
    "details": {
      "type": "utility"
    }
  }
}
```

## **Responses**

| HTTP Code | Description                                  | Response Body           |
|-----------|----------------------------------------------|-------------------------|
| 200       | Security token successfully created          | `CreateStResponse`      |
| 400       | Bad request (e.g., invalid input)            | `Error` object          |
| 500       | Internal server error                        | `Error` object          |

### **Example Response (200)**

```json
{
  "st_id": "issuer123-MyToken"
}
```

### **Field Descriptions**

- **st_id** *(String)*: Unique identifier for the created security token.

### **Error Responses**

#### **Bad Request (400)**
```json
{
  "error": "Issued amount must be positive"
}
```

#### **Internal Server Error (500)**
```json
{
  "error": "An unexpected error occurred"
}
```


### **Procedure**
1. **Frontend:** Initiates a POST request with `issuerId`, `tokenName`, `amount`, and `info`.
2. **Controller:** Validates the request and forwards it to the service layer.
3. **Service:**
   - Validates data against ledger state.
   - Ensures no conflicts (e.g., duplicate `st_id`, invalid `issuerId`).
4. **Ledger:** Confirms the validity of `issuerId` and checks for existing `st_id` conflicts.
5. **Database:** Creates the token entry and updates the issuer’s account balances.
6. **Service:** Returns success response with `st_id` or propagates errors to the controller.
7. **Controller:** Sends appropriate HTTP response to the frontend.

---

