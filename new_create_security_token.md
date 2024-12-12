
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
    alt Invalid issuer_id or Duplicate st_id Found
        Service -->> Controller: HTTP 400 Bad Request<br/>Validation Error
        Controller -->> Client: HTTP 400 Bad Request<br/>Invalid issuer_id or Duplicate st_id
    else Valid Request Body
        Service ->> Scalardl: Create Security Token
        Scalardl -->> Service: Token Creation Confirmed
        Service -->> Controller: HTTP 201 Created<br/>Success with st_id
        Controller -->> Client: HTTP 201 Created<br/>with st_id
    end
