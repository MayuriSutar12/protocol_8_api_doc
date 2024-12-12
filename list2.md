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
    Database -->> Service: Return List of STs (may be empty)
    Service -->> Controller: Return List of STs (may be empty)
    Controller -->> Client: HTTP 200 OK<br/>with ST list and pagination (empty if no tokens)

