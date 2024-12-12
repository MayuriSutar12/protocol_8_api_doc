### Sample Sequence Diagram

```mermaid
sequenceDiagram
    participant Client as Client
    participant Controller as Controller
    participant Service as Service
    participant Database as Database

    Client ->> Controller: GET /security-tokens<br/>with limit and offset (optional)
    Controller ->> Service: Retrieve Resources (limit, offset)
    Service ->> Database: Query Resources with limit and offset
    alt No Resources Found
        Database -->> Service: No Results Found
        Service -->> Controller: Empty List
        Controller -->> Client: HTTP 404 Not Found<br/>No Resources Found
    else Resources Found
        Database -->> Service: List of Resources
        Service -->> Controller: List of Resources
        Controller -->> Client: HTTP 200 OK<br/>with Resource List
    end
