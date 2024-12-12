### Sample Sequence Diagram

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

