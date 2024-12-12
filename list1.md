### Sample Sequence Diagram

```mermaid
sequenceDiagram
    participant Client as Client
    participant Controller as Controller
    participant Service as Service
    participant Database as Database

    Client ->> Controller: GET /security-tokens<br/>with limit and offset (optional)
    Controller ->> Service: Retrieve Resources (limit, offset)
    Service ->> Database: Fetch STs List
        Controller -->> Client: HTTP 404 Not Found<br/>Error responce
        Database -->> Service: Return List of STs
        Service -->> Controller: Return List of STs
        Controller -->> Client: HTTP 200 OK<br/>with ST list with pagination
    end
