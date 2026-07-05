# 📘 API Interview Notes (Interview Ready)

---

## Can you explain what an API is?

API stands for Application Programming Interface. It is a set of rules that allows two software applications to communicate with each other. A client sends a request to an API, and the API communicates with the server and returns the appropriate response. APIs help applications exchange data and functionality without exposing their internal implementation.

---

## What's the difference between an API and a REST API?

An API is a general concept that allows different software applications to communicate with each other. A REST API is a type of API that follows REST architectural principles. REST APIs typically use HTTP methods such as GET, POST, PUT, PATCH, and DELETE. They are stateless, meaning each request contains all the information needed to process it. REST APIs commonly exchange data in JSON format, although other formats like XML are also supported.

---

## What is the difference between HTTP and HTTPS?

HTTP stands for Hypertext Transfer Protocol, while HTTPS stands for Hypertext Transfer Protocol Secure. The main difference is that HTTPS encrypts the communication between the client and the server using SSL/TLS, making the data secure during transmission. HTTP sends data in plain text, so it is vulnerable to interception. The default port for HTTP is 80, while HTTPS uses port 443. Today, HTTPS is the standard for websites and APIs because it provides encryption, authentication, and data integrity.

---

## What do you mean by a stateless REST API?

A stateless REST API means that each client request is independent and contains all the information needed for the server to process it. The server does not store the client's session state between requests. If the client makes another request, it must again send any required information, such as an authentication token.

---

## Can you explain the difference between the HTTP methods:

### Interview-Ready Answer

| Method | Purpose | Example |
|--------|---------|---------|
| GET | Retrieve data | Get user details |
| POST | Create a new resource | Create a new user |
| PUT | Replace the entire resource | Replace all user information |
| PATCH | Partially update a resource | Update only the user's email |
| DELETE | Remove a resource | Delete a user |

---

## What is the difference between an HTTP status code 200, 201, 400, 401, 403, 404, and 500?

| Status Code | Meaning | Example |
|-------------|---------|---------|
| **200 OK** | Request processed successfully | GET `/users/1` |
| **201 Created** | New resource created successfully | POST `/users` |
| **400 Bad Request** | Invalid request from the client | Missing required field |
| **401 Unauthorized** | Authentication is missing or invalid | Invalid or missing token |
| **403 Forbidden** | Authenticated, but not allowed to access the resource | Regular user trying to access an admin endpoint |
| **404 Not Found** | Requested resource doesn't exist | User ID not found |
| **500 Internal Server Error** | Unexpected server-side error | Database crash or unhandled exception |

---

## What is JSON? Why is it used in REST APIs?

JSON stands for JavaScript Object Notation. It is a lightweight, text-based data format used to exchange data between a client and a server. REST APIs commonly use JSON because it is human-readable, easy to parse, language-independent, and produces smaller payloads than XML.

A JSON object is enclosed in curly braces {} and contains key-value pairs, while a JSON array is enclosed in square brackets [] and contains a list of values or objects.

---

## What is the difference between Authentication and Authorization?

Authentication is the process of verifying a user's identity. It answers the question, **"Who are you?"** This is typically done using a username and password, OTP, or an access token.

Authorization is the process of determining what an authenticated user is allowed to access or perform. It answers the question, **"What are you allowed to do?"**

Authentication always happens before authorization.

---

## What is the difference between a Path Parameter and a Query Parameter?

Path parameters are part of the URL path and are used to identify a specific resource. For example, in `GET /users/101`, `101` is a path parameter that identifies the user.

Query parameters are appended after a `?` in the URL and are typically used for filtering, searching, sorting, or pagination. For example, `GET /users?country=India&age=22` filters users based on the given criteria.

Path parameters identify which resource you want, while query parameters specify how you want the results.

---

## What is idempotency in REST APIs?

An idempotent API operation is one where making the same request multiple times results in the same final state on the server as making it once. Idempotency is about the effect on the server, not necessarily returning the exact same response every time.

For example, GET, PUT, and DELETE are idempotent, while POST is generally not because repeated POST requests may create multiple resources.

---

# High-Priority Topics (Must Know)

You should be comfortable with:

- ✅ What is an API?
- ✅ API vs REST API
- ✅ REST principles
- ✅ HTTP vs HTTPS
- ✅ HTTP methods (GET, POST, PUT, PATCH, DELETE)
- ✅ Idempotency
- ✅ Status codes (200, 201, 204, 400, 401, 403, 404, 409, 500)
- ✅ JSON
- ✅ Path parameters vs Query parameters
- ✅ Authentication vs Authorization
- ✅ Headers (Authorization, Content-Type, Accept)
- ✅ Statelessness
- ✅ Request and Response structure

---

# Medium-Priority Topics ⭐⭐⭐⭐☆

These are asked very frequently in product companies:

- API versioning
- Pagination (limit, offset, page, size)
- Sorting and filtering
- API documentation (such as Swagger UI / OpenAPI Specification)
- CRUD operations
- URI vs URL
- Endpoint vs Resource
- Request body vs Query parameters
- Cookies vs Tokens
- Session-based vs Token-based authentication
- Common HTTP headers

---

# Advanced Topics (Good to Know) ⭐⭐⭐☆☆

- Caching
- Rate limiting
- CORS
- OAuth 2.0 (basic idea)
- JWT (basic structure and use)
- API Gateway
- Microservices and APIs
- Webhooks
- SOAP vs REST
- REST vs GraphQL
