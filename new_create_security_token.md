
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
    Service ->> Scalardl: Create Security Token
    alt
        Scalardl -->> Service: Error Response
        Service -->> Controller:  Invalid issuer_id or Duplicate st_id Found
        Controller -->> Client: Error Response<br/>Validation Error or Duplicate st_id
    else Token Creation Success
        Scalardl -->> Service: Success Response
        Service -->> Controller: Success Response with st_id
        Controller -->> Client: Success Response with st_id
    end

