### Sample Sequence Diagram

```mermaid
sequenceDiagram
    participant Client as Client
    participant Controller as Controller
    participant Service as Service
    participant Database as Database

    Client ->> Controller: GET /security-tokens<br/>with limit and offset (optional)
    Controller ->> Service: Fetch STs List with limit and offset
    Service ->> Database: Fetch STs List
    alt 

        Service -->> Controller: Empty List
        Controller -->> Client: HTTP 400 Bad Request<br/>Error Response
       Service -->> Controller: Error Response
        Controller -->> Client:HTTP 400 Not Found
    else Security Tokens Found
        Database -->> Service: Return List of STs
        Service -->> Controller: List of STs
        Controller -->> Client: HTTP 200 OK<br/>with ST list and pagination
    end

