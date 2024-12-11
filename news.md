### Sample Sequence Diagram
Here is a customizable sequence diagram format using Mermaid notation. Change participant roles and message interactions according to your specifications.
sequenceDiagram


```mermaid
sequenceDiagram
    participant Client as Client
    participant Controller as Controller
    participant Service as Service
    participant Scalardl as Scalardl
    participant Database as Database

    Client ->> Controller: GET /security-tokens<br/>with limit and offset (optional)
    Controller ->> Service: Validate Query Parameters (limit, offset)
    alt Validation Failed
        Service -->> Controller: HTTP 400 Bad Request<br/>Invalid Query Parameters
        Controller -->> Client: HTTP 400 Bad Request<br/>Invalid Query Parameters
    else Validation Succeeded
        Service ->> Scalardl: Validate Data Integrity and Authorization
         Scalardl->> Database: Validate Data Integrity and Authorization
        alt Scalardl Validation Failed
            Scalardl -->> Service: Validation Error
            Database -->> Scalardl: Validation Error
            Service -->> Controller: HTTP 400 Bad Request<br/>Validation Error
            Controller -->> Client: HTTP 400 Bad Request<br/>Validation Error
        else Scalardl Validation Passed
            Service ->> Database: Fetch STs List<br/>
            alt No Security Tokens Found
                Database -->> Service: No Results Found
                Service -->> Controller: HTTP 404 Not Found<br/>No Security Tokens
                Controller -->> Client: HTTP 404 Not Found<br/>No Security Tokens
            else Security Tokens Found
                Database -->> Service: Return List of STs
                Service -->> Controller: HTTP 200 OK<br/>with ST list with pagination
                Controller -->> Client: HTTP 200 OK<br/>with ST list with pagination
            end
        end
  

