
### Sample Sequence Diagram
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
        Scalardl -->> Scalardl: if duplicate entry
        Scalardl -->> Service: Error Response
        Service -->> Controller: Invalid issuer_id or Duplicate st_id Found
        Controller -->> Client: Error Response
    else Token Creation Success
        Scalardl -->> Service: Success Response with st_id
        Service -->> Controller: Success Response with st_id
        Controller -->> Client: Success Response 
    end
