# Gateway
HTTP interface.   
A connection each different protocol and application.   
Protocol translation.   
Path Guide.   
Connect resource and application.   


- A gateway is a piece of networking hardware or software
- A gateway is used in telecommunications networks.
- A gateway can operate at any of the seven layers of the OSI model.
- Ex. connects an office/home intranet to the internet.
    -> load a web page -> at least two network gateways are accessed.
    (to get from the office/home network to the internet + to get from the internet to the computer that serves the web pages)

## Gateway Client & Gateway Server
<Client Protocol\>/<Server Protocol\>   
Ex. HTTP/NNTP gateway.

### Ex. A vital role of the payment gateway
1. Payment Data Encryption
2. Authorization and Verification
3. Transaction Routing
4. Transaction Status Updates
5. (facilitates) Settlement and Funds Transfer

The payment gateway != A payment processor   
A payment gateway is the front-end technology that collects and sends the customer information to the payment processor.

### Then what is the gateway of PayPal?
- A payment gateway.
- sends credit card information 
- from a website to the credit card payment networks

### Then Is Google Authorization a gateway?
No, It's not a gateway. It's an authorization protocol.
- Protocol: OAuth 2.0
- Authentication: Verifies the user's identity
- Authorization: Grants the application permission to access specific data or perform actions on behalf of the user.
- Flow: The user grants permmission via a Google login -> The application gets an OAuth access token -> access the user's data or verify their identity.

### What is the difference between API and Gateway
API (Application Programming Interface):   
- An API is a set of rules and tools that allows one piece of software to communicate with another. It defines how requests and responses should be structured, what data can be exchanged, and how systems interact.
- Example: A weather API provides an interface for a developer to request weather data from a service.
- APIs provide direct access to a system's functionality or data.
- They define the specific endpoints (URLs), methods (GET, POST, PUT, DELETE), and data formats (JSON, XML) that a client can use to interact with a backend service.
- APIs typically deal with individual operations like retrieving data, creating a record, or updating information.

Gateway:   
- A gateway is a mediator or intermediary that controls the flow of data between different systems or services. - It handles traffic management, security, and communication between clients (like web browsers or mobile apps) and backend services (like APIs, databases, or other microservices).
- Example: An API gateway manages API requests from clients, routes them to the correct API, handles authentication, rate limiting, and more.
- A gateway is like a traffic controller that routes API requests and manages multiple APIs. It is responsible for handling tasks such as authentication, rate limiting, caching, logging, and load balancing.
- It provides a unified entry point for accessing multiple APIs or services, thus simplifying how clients interact with multiple services.
- Gateways add an extra layer of abstraction between the client and backend services.

--- 
### Interpreter
- a computer program that directly execute instructions written in a programming or scripting lauguage.
- don't need to have been compiled into a machine language program.



[Reference]('https://en.wikipedia.org/wiki/Gateway_(telecommunications)')