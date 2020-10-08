# REST API Cheat Sheet

## Quick Facts

 * REST stands for **RE**presentational **S**tate **T**ransfer. 
 * REST is an **architecture**, not a framework or protocol. As such, it is a set of guidelines and constraints rather than an international standard.
 * REST is **stateless**, meaning that _all_ requests should contain all of the necessary information to complete the request.
 * REST is built on top of the **Hypertext Transfer Protocol (HTTP)**, meaning that it uses a system of **requests** and **responses**
   * **Requests** are sent from the client to the server and include an **HTTP method**, **request headers**, and a **request body** (optional).
   * **Responses** are sent from the server to the client to reply to a request and include an **HTTP status code**, **response headers**, and a **response body** (optional).
 * REST APIs are **resource**-based. These resources are then organized into **collections**. Some **operations** (such as reading, modifying, deleting) only should act on a single resource while others (reading, creating) should act on collections.
 * Resources and collections can be access from certain **endpoints**. Endpoint naming conventions are as follows:
   * Accessing _collections_: `${API_URL}/resources` (e.g., `${API_URL}/users`)
   * Accessing _resources_: `${API_URL}/resources/${resource_id}` (e.g. `${API_URL/users/123456}`)
   
## "Big Four (Five)" HTTP Methods

The following HTTP methods correspond to the **CRUD** operations of **create**, **read**, **update**, **delete**.
Note also the distinction between **safe** and **idempotent** methods:
 * **Idempotent** methods are those which can be repeated without consequence. In other words, repeating the request _n_ times will produce the same result as only issuing it once.
 * **Safe** methods are those only intended for _retrieving_ information. As such, safe methods are by their nature idempotent.
 * A method _can_ be neither safe nor idempotent.

| HTTP Method | CRUD | Operating on Collections<br/>(e.g., `/users`) | Operating on Individual Resources<br/>(e.g., `/users/123456`) |
| :---: | :---: | :--- | :--- |
| **POST**  | Creating  | **201 (Created)**: if creating was successful. <br/>Use `Location` header to give client a link to access that indidivual resource (e.g., `/users/123`). | _Not recommended_<br/>**404 (Not Found)** or **409 (Conflict)**: if resource already exists. |
| **GET**<br/>(safe)  | Reading | **200 (OK)**: provide list of resources | **200 (OK)**: provide single resource<br/>**404 (Not Found)** if ID was not found or invalid |
| **PUT**<br/>(idempotent) | Updating<br/>(Replacing) | **405 (Method Not Allowed)**: unless you support updating _every_ resource in the collection | **200 (OK)** or **204 (No Content)**: success<br/>**404 (Not Found)** if ID was not found or invalid |
| **_PATCH*_** | Updating<br/>(Modifying) | **405 (Method Not Allowed)**: unless you support modifying the **collection** itself | **200 (OK)** or **204 (No Content)**: success<br/>**404 (Not Found)** if ID was not found or invalid |
| **DELETE**<br/>(idempotent) | Deleting | **405 (Method Not Allowed)**: unless you support deleting the entire collection | **200 (OK)**: success<br/>**404 (Not Found)** if ID was not found or invalid |

Some general HTTP response codes:
 * **400 (Bad Request)**: client send invalid data
 * **401 (Unauthorized)**: client does not have permission to access resource.
 * **403 (Forbidden)**: client does not have permission to access resouce AND authentication will not help.
 * **405 (Method Not Allowed)**: the HTTP method in the request is not supported
 * **500 (Internal Server Error)**: catch-all server error (usually you don't want to intentionally respond with this code.

\* Note that `PATCH` is not used often but still is a perfectly valid HTTP method to use.

## Useful HTTP Status Codes

What follows is a list of _some_ of the commonly-used HTTP status codes used in RESTful services.

| Status Code | Name | Short Description | Tips |
| :---: | :---: | :--- | :--- |
| 200 | OK | The request has suceeded. | Use to indicate success. |
| 201 | Created | The request has suceeded and a new resource has been created. | Use to indicate successful creation via POST or PUT. |
| 204 | No Content | The request has suceeded but nothing needs to be returned to client. | Use to indicate success but nothing is needed to be sent back to client (e.g., DELETE). No body on response. |
| 304 | Not Modified | The client performed a conditional GET request and access is allowed but the document has not been modified. | Use to indicate the the resource has not changed since the last time accessed. Not body on response. |
| 400 | Bad Request | The request was not understood by the server. | Use when the client send bad data (e.g., syntax error, bad data type, etc.). |
| 401 | Unauthorized | The request requires user authentication. | Use when request is missing or has an invalid authentication token. | 
| 403 | Forbidden | The server understood the request, but is refusing to fulfill it. Authorization will not help. | Use when user not authorized to perform the operation or the resource is unavailable for some reason. | 
| 404 | Not Found | The server has not found anything matching the request URI. | Use when the requested resource does not exist. |
| 405 | Method Not Allowed | The method specified in the request is not allowed for the resource. | Use when the method specified in the request is not allowed for the resource. |
| 409 | Conflict | The request could not be completed due to a conflict with the current state of the resource. The user may be able to resolve the conflict on their end. | Use when fufilling the request would cause a conflict or invalid state (e.g., a user with that email already exists, deleting the root of a tree, etc.). |
| 500 | Internal Server Error | The server encountered an unexpected condition which prevented it from fulfilling the request. | Use when an error occurred on the server (perhaps with some addition information on what happened). | 
| 501 | Not Implemented | The server does not support the functionality required to fulfill the request. | Use when the request _would_ have worked, but the functionality is not yet implemented.
| 503 | Service Unavailable | The server is currently unable to handle the request due to a temporary overloading or maintenance of the server. | Use when the server cannot handle the request for some temporary period of time. | 
