# UNIT IV - Building RESTful APIs and MongoDB

## Table of Contents

1. [Building RESTful APIs](#1-building-restful-apis)
   - [What is REST?](#what-is-rest)
   - [REST Principles](#rest-principles)
   - [REST Constraints](#rest-constraints)
   - [Resource-Based Architecture](#resource-based-architecture)
   - [Statelessness](#statelessness)

2. [HTTP Methods as Actions](#2-http-methods-as-actions)
   - [HTTP Methods Mapping](#http-methods-mapping)
   - [Detailed Method Explanations](#detailed-method-explanations)
   - [Idempotency](#idempotency)
   - [Safe vs Unsafe Methods](#safe-vs-unsafe-methods)
   - [REST Flexibility](#rest-flexibility)

3. [JSON (JavaScript Object Notation)](#3-json-javascript-object-notation)
   - [What is JSON?](#what-is-json)
   - [JSON Structure](#json-structure)
   - [JSON vs XML](#json-vs-xml)
   - [JSON in REST APIs](#json-in-rest-apis)

4. [Express Framework Basics](#4-express-framework-basics)
   - [Introduction to Express](#introduction-to-express)
   - [Why Express for REST APIs?](#why-express-for-rest-apis)
   - [Express Application Setup](#express-application-setup)
   - [Routing Handler Functions](#routing-handler-functions)
   - [Express Folder Structure](#express-folder-structure)
   - [Serving Static Files](#serving-static-files-in-express)

5. [Routing in Express](#5-routing-in-express)
   - [Route Definitions](#route-definitions)
   - [Request Matching](#request-matching)
   - [Route Parameters](#route-parameters)
   - [Query Parameters](#query-parameters)
   - [Modular Routing](#modular-routing)
   - [REST-style Route Design](#rest-style-route-design)
   - [Route Lookup Order](#route-lookup-order)

6. [Request and Response Objects](#6-request-and-response-objects)
   - [Request Object (req)](#request-object-req)
   - [Response Object (res)](#response-object-res)
   - [Common HTTP Status Codes](#common-http-status-codes)
   - [Method Chaining](#method-chaining)

7. [Middleware in Express](#7-middleware-in-express)
   - [What is Middleware?](#what-is-middleware)
   - [Middleware Function Structure](#middleware-function-structure)
   - [Types of Middleware](#types-of-middleware)
   - [Middleware Execution Order](#middleware-execution-order)
   - [The next() Function](#the-next-function)
   - [Middleware Pipeline Flow](#middleware-pipeline-flow)
   - [Authentication Middleware Example](#authentication-middleware-example)
   - [Logging Middleware Example](#logging-middleware-example)
   - [Error Handling Middleware](#error-handling-middleware)

8. [REST APIs with MongoDB](#8-rest-apis-with-mongodb)
   - [Connecting Express with MongoDB](#connecting-express-with-mongodb)
   - [API-Database Interaction Flow](#api-database-interaction-flow)
   - [Data Flow in REST APIs](#data-flow-in-rest-apis)

9. [The LIST API](#9-the-list-api)
   - [Purpose of LIST API](#purpose-of-list-api)
   - [REST Endpoint Design](#rest-endpoint-design)
   - [Controller Logic](#controller-logic)
   - [Database Interaction](#database-interaction)
   - [Response Structure](#response-structure)
   - [Complete LIST API Example](#complete-list-api-example)

10. [The CREATE API](#10-the-create-api)
    - [Purpose of CREATE API](#purpose-of-create-api)
    - [POST Request Handling](#post-request-handling)
    - [Validation](#validation)
    - [Database Insert Logic](#database-insert-logic)
    - [Response Handling](#response-handling)
    - [Complete CREATE API Example](#complete-create-api-example)

11. [Using the LIST API](#11-using-the-list-api)
    - [Client Request Example](#client-request-example)
    - [Server Processing Flow](#server-processing-flow)
    - [JSON Response Example](#json-response-example)

12. [Using the CREATE API](#12-using-the-create-api)
    - [Client Request Payload](#client-request-payload)
    - [Server-side Processing](#server-side-processing)
    - [Error Cases](#error-cases)
    - [Success Response](#success-response)

13. [Error Handling in REST APIs](#13-error-handling-in-rest-apis)
    - [Types of Errors](#types-of-errors)
    - [HTTP Status Codes](#http-status-codes)
    - [Centralized Error Handling](#centralized-error-handling)
    - [Error Middleware](#error-middleware)
    - [Best Practices for API Error Responses](#best-practices-for-api-error-responses)
    - [Async Error Handling Helper](#async-error-handling-helper)
    - [Complete Error Handling Example](#complete-error-handling-example)

14. [GraphQL](#14-graphql)
    - [Introduction to GraphQL](#introduction-to-graphql)
    - [Key Components of GraphQL](#key-components-of-graphql)
    - [GraphQL Schema Design Example](#graphql-schema-design-example)
    - [GraphQL Resolvers](#graphql-resolvers)
    - [GraphQL Server Setup](#graphql-server-setup)
    - [GraphQL API Integration with React JS](#graphql-api-integration-with-react-js)
    - [GraphQL Error Handling](#graphql-error-handling)
    - [GraphQL vs REST Comparison](#graphql-vs-rest-comparison)

15. [Important Diagrams](#important-diagrams)
    - [REST Request-Response Lifecycle](#rest-request-response-lifecycle)
    - [Express Middleware Pipeline](#express-middleware-pipeline)
    - [REST API Data Flow with MongoDB](#rest-api-data-flow-with-mongodb)
    - [CREATE API Flow](#create-api-flow)
    - [GraphQL Architecture](#graphql-architecture)

16. [20 Important Exam Questions for UNIT IV](#20-important-exam-questions-for-unit-iv)
    - [Short Answer Questions](#short-answer-questions-2-5-marks)
    - [Long Answer Questions](#long-answer-questions-8-10-marks)
    - [Code-Based Questions](#code-based-questions-10-marks)
    - [Diagram-Based Questions](#diagram-based-questions-5-8-marks)

17. [Revision Tips for Maximum Marks](#revision-tips-for-maximum-marks)

18. [Quick Reference Tables](#quick-reference-tables)
    - [HTTP Methods Quick Reference](#http-methods-quick-reference)
    - [Express Request Object Properties](#express-request-object-properties)
    - [HTTP Status Codes](#http-status-codes)

---

## 1. Building RESTful APIs

### What is REST?

REST (Representational State Transfer) is an architectural pattern for application programming interfaces (APIs). It defines a set of constraints and principles for creating web services that are scalable, maintainable, and easy to understand.

### REST Principles

REST is based on the following core principles:

1. **Client-Server Architecture**: Separation of concerns between client and server
2. **Statelessness**: Each request contains all information needed to process it
3. **Cacheability**: Responses must define themselves as cacheable or non-cacheable
4. **Uniform Interface**: Standardized way of communicating between client and server
5. **Layered System**: Architecture can be composed of hierarchical layers
6. **Code on Demand** (optional): Servers can extend client functionality

### REST Constraints

#### Resource-Based Architecture

- APIs are resource-based as opposed to action-based
- API names like `getSomething` or `saveSomething` are not normal in REST APIs
- There are no API names in the conventional sense
- APIs are formed by a combination of resources and actions
- Only resource names called **endpoints** exist

**Resources and URIs:**
- Resources are accessed based on a Uniform Resource Identifier (URI), also known as an endpoint
- URIs per resource:
  - One for the collection: `/customers`
  - One for an individual object: `/customers/1234`
  - The identifier (1234) uniquely identifies a customer

#### Statelessness

Statelessness means that each request from a client to a server must contain all the information needed to understand and process the request. The server does not store any client context between requests.

**Characteristics:**
- No session information stored on server
- Each request is independent
- All state information is contained in the request itself
- Authentication/authorization data sent with each request

**Benefits:**
- Improved scalability
- Simplified server design
- Better reliability
- Easier to cache responses

**Example:**
```javascript
// Stateless request - includes all necessary information
GET /api/users/123
Headers:
  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
  Content-Type: application/json
```

## 2. HTTP Methods as Actions

Resources are nouns, and HTTP methods are verbs that operate on them. They map to CRUD (Create, Read, Update, Delete) operations on the resource.

### HTTP Methods Mapping

| HTTP Method | CRUD Operation | Purpose | Idempotent | Safe |
|-------------|----------------|---------|------------|------|
| GET | Read | Retrieve resource(s) | Yes | Yes |
| POST | Create | Create new resource | No | No |
| PUT | Update/Replace | Replace entire resource | Yes | No |
| PATCH | Update/Modify | Partially update resource | No | No |
| DELETE | Delete | Remove resource | Yes | No |

### Detailed Method Explanations

#### GET
- Used to retrieve data from the server
- Does not modify any data
- Can be cached
- Parameters sent in URL query string

**Example:**
```javascript
GET /api/customers
GET /api/customers/1234
GET /api/customers?city=Bangalore&status=active
```

#### POST
- Used to create new resources
- Sends data in request body
- Not idempotent (multiple requests create multiple resources)
- Returns created resource with status 201

**Example:**
```javascript
POST /api/customers
Body: {
  "name": "John Doe",
  "email": "john@example.com"
}
```

#### PUT
- Used to replace an entire resource
- Sends complete updated data in request body
- Idempotent (multiple identical requests have same effect)
- Creates resource if it doesn't exist

**Example:**
```javascript
PUT /api/customers/1234
Body: {
  "name": "John Doe",
  "email": "john.doe@example.com",
  "city": "Bangalore"
}
```

#### PATCH
- Used to partially update a resource
- Sends only fields to be updated
- Not necessarily idempotent
- More efficient than PUT for small updates

**Example:**
```javascript
PATCH /api/customers/1234
Body: {
  "email": "newemail@example.com"
}
```

#### DELETE
- Used to remove a resource
- Idempotent (deleting same resource multiple times has same effect)
- May return 204 No Content or 200 OK

**Example:**
```javascript
DELETE /api/customers/1234
```

### Idempotency

An operation is idempotent if performing it multiple times has the same effect as performing it once.

**Idempotent Methods:** GET, PUT, DELETE
**Non-Idempotent Methods:** POST, PATCH

### Safe vs Unsafe Methods

**Safe Methods:**
- Do not modify resources
- Only retrieve data
- Example: GET

**Unsafe Methods:**
- Modify or delete resources
- Change server state
- Examples: POST, PUT, PATCH, DELETE

### REST Flexibility

REST by itself lays down no rules for the following:

1. **Filtering, sorting, and paginating**: Query string is commonly used
   - Example: `/api/issues?page=2&limit=10&sort=createdAt`
2. **Specifying fields to return**: Implementation-specific
   - Example: `/api/users?fields=name,email`
3. **Embedded objects expansion**: Which nested objects to include
4. **Fields to modify in PATCH**: Which fields are updatable
5. **Representation of objects**: JSON, XML, or any other format

## 3. JSON (JavaScript Object Notation)

### What is JSON?

JSON (JavaScript Object Notation) is a lightweight data interchange format that is easy for humans to read and write, and easy for machines to parse and generate.

### JSON Structure

JSON is built on two structures:

1. **Object**: Collection of name-value pairs
2. **Array**: Ordered list of values

**Syntax Rules:**
- Data is in name-value pairs
- Data is separated by commas
- Curly braces hold objects
- Square brackets hold arrays
- Names must be strings in double quotes
- Values can be: string, number, object, array, boolean, null

**Example:**
```json
{
  "name": "John Doe",
  "age": 30,
  "email": "john@example.com",
  "isActive": true,
  "address": {
    "street": "123 Main St",
    "city": "Bangalore"
  },
  "hobbies": ["reading", "coding", "gaming"]
}
```

### JSON vs XML

| Feature | JSON | XML |
|---------|------|-----|
| Syntax | Lightweight, less verbose | More verbose with tags |
| Data Types | Supports numbers, strings, booleans, arrays, objects, null | Everything is text |
| Parsing | Faster and easier | Slower parsing |
| Readability | More readable | Less readable due to tags |
| Size | Smaller file size | Larger file size |
| Arrays | Native array support | No native array support |
| Namespaces | No namespace support | Supports namespaces |
| Comments | No comments | Supports comments |

**JSON Example:**
```json
{
  "customer": {
    "id": 1234,
    "name": "John Doe"
  }
}
```

**XML Example:**
```xml
<customer>
  <id>1234</id>
  <name>John Doe</name>
</customer>
```

### JSON in REST APIs

#### Request Payloads

JSON is used to send data to the server in POST, PUT, and PATCH requests.

**Example POST Request:**
```javascript
POST /api/customers
Content-Type: application/json

{
  "name": "Jane Smith",
  "email": "jane@example.com",
  "phone": "9876543210"
}
```

#### Response Payloads

JSON is the standard format for API responses.

**Example Response:**
```json
{
  "status": "success",
  "data": {
    "id": 1234,
    "name": "Jane Smith",
    "email": "jane@example.com",
    "createdAt": "2025-12-21T10:30:00Z"
  }
}
```

## 4. Express Framework Basics

### Introduction to Express

Express is a minimal and flexible Node.js web application framework that provides a robust set of features for building web and mobile applications.

**Key Features:**
- Lightweight and fast
- Robust routing
- Middleware support
- Template engine integration
- Easy to learn and use

### Why Express for REST APIs?

1. **Minimal yet Flexible**: Express by itself does very little, focusing on core web application capabilities
2. **Middleware Architecture**: Add features as needed using middleware
3. **Routing Capabilities**: Powerful routing mechanism for handling different endpoints
4. **Large Ecosystem**: Extensive collection of middleware and plugins
5. **Performance**: Fast and efficient
6. **Community Support**: Large community and extensive documentation

### Express Application Setup

**Basic Setup:**

```javascript
// Import Express module
const express = require('express');

// Create Express application
const app = express();

// Define port
const port = 3000;

// Basic route
app.get('/', (req, res) => {
  res.send('Hello World!');
});

// Start server
app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
```

**Installation:**
```bash
npm install express
```

### Routing Handler Functions

The main part of Express is a router that:
- Takes a client request
- Matches it against routes that are present
- Executes the handler function associated with that route

**Handler Function Structure:**
```javascript
app.get('/hello', (req, res) => {
  res.send('Hello World!');
});
```

The handler function receives:
- **req**: Request object (contains request details)
- **res**: Response object (used to send response)

### Express Folder Structure

A typical Express application follows a logical folder structure:

```
project-root/
│
├── node_modules/          // Dependencies
├── public/                // Static files (CSS, JS, images)
├── routes/                // Route definitions
│   ├── users.js
│   └── products.js
├── models/                // Database models
│   └── User.js
├── controllers/           // Route handler logic
│   └── userController.js
├── middleware/            // Custom middleware
│   └── auth.js
├── config/                // Configuration files
│   └── database.js
├── views/                 // Template files (if using)
├── app.js                 // Main application file
├── package.json           // Project metadata
└── .env                   // Environment variables
```

### Serving Static Files in Express

Express can serve static files like images, CSS, and JavaScript.

```javascript
const express = require('express');
const app = express();

// Serve static files from 'public' directory
app.use(express.static('public'));

app.listen(3000);
```

This serves all files inside the 'public' directory at the root path.

**Example:**
- File at `public/css/style.css` is accessible at `http://localhost:3000/css/style.css`

## 5. Routing in Express

### Route Definitions

A route specification consists of:
1. **HTTP method** (GET, POST, PUT, DELETE, etc.)
2. **Path specification** that matches the request URI
3. **Route handler function**

**Basic Route Structure:**
```javascript
app.METHOD(PATH, HANDLER)
```

**Examples:**
```javascript
// GET route
app.get('/users', (req, res) => {
  res.json({ message: 'Get all users' });
});

// POST route
app.post('/users', (req, res) => {
  res.json({ message: 'Create user' });
});

// PUT route
app.put('/users/:id', (req, res) => {
  res.json({ message: `Update user ${req.params.id}` });
});

// DELETE route
app.delete('/users/:id', (req, res) => {
  res.json({ message: `Delete user ${req.params.id}` });
});
```

### Request Matching

The route's method and path need not be specific.

**Match All HTTP Methods:**
```javascript
app.all('/secret', (req, res) => {
  res.send('Accessing secret section');
});
```

**Match Multiple Paths:**
```javascript
// Array of paths
app.get(['/users', '/customers'], (req, res) => {
  res.send('Users or Customers');
});

// Regular expression
app.get('/*.do', (req, res) => {
  res.send('Matches any request ending with .do');
});
```

### Route Parameters

Route parameters are named segments in the path specification that match a part of the URL.

**Syntax:**
```javascript
app.get('/customers/:customerId', (req, res) => {
  const id = req.params.customerId;
  res.send(`Customer ID: ${id}`);
});
```

**Examples:**
- URL `/customers/1234` → `req.params.customerId = "1234"`
- URL `/customers/4567` → `req.params.customerId = "4567"`

**Multiple Parameters:**
```javascript
app.get('/users/:userId/posts/:postId', (req, res) => {
  const { userId, postId } = req.params;
  res.json({ userId, postId });
});
```

### Query Parameters

Query parameters are passed in the URL after the `?` symbol.

**Example URL:**
```
/api/products?category=electronics&price=500&sort=asc
```

**Access in Express:**
```javascript
app.get('/api/products', (req, res) => {
  const { category, price, sort } = req.query;
  res.json({ category, price, sort });
});
```

**Comparison:**

| Feature | Route Parameters | Query Parameters |
|---------|------------------|------------------|
| Position | Part of URL path | After ? in URL |
| Purpose | Identify resource | Filter/sort/paginate |
| Required | Usually required | Optional |
| Example | `/users/123` | `/users?role=admin` |
| Access | `req.params` | `req.query` |

### Modular Routing

For larger applications, routes can be organized into separate modules.

**users.js (Route file):**
```javascript
const express = require('express');
const router = express.Router();

// Define routes
router.get('/', (req, res) => {
  res.json({ message: 'Get all users' });
});

router.get('/:id', (req, res) => {
  res.json({ message: `Get user ${req.params.id}` });
});

router.post('/', (req, res) => {
  res.json({ message: 'Create user' });
});

module.exports = router;
```

**app.js (Main file):**
```javascript
const express = require('express');
const app = express();
const userRoutes = require('./routes/users');

// Use modular routes
app.use('/api/users', userRoutes);

app.listen(3000);
```

### REST-style Route Design

RESTful route design follows consistent patterns:

| Operation | HTTP Method | Endpoint | Purpose |
|-----------|-------------|----------|---------|
| List all | GET | `/api/customers` | Get all customers |
| Get one | GET | `/api/customers/:id` | Get specific customer |
| Create | POST | `/api/customers` | Create new customer |
| Update | PUT | `/api/customers/:id` | Replace customer |
| Partial Update | PATCH | `/api/customers/:id` | Update customer fields |
| Delete | DELETE | `/api/customers/:id` | Delete customer |

### Route Lookup Order

Multiple routes can be set up to match different URLs and patterns.

**Important Rule:** More specific routes should be defined before generic ones.

**Example:**
```javascript
// Specific route (define first)
app.get('/api/issues/new', (req, res) => {
  res.send('New issue form');
});

// Generic route with parameter (define after)
app.get('/api/issues/:id', (req, res) => {
  res.send(`Issue ${req.params.id}`);
});

// Catch-all route (define last)
app.get('/api/*', (req, res) => {
  res.send('API catch-all');
});
```

## 6. Request and Response Objects

### Request Object (req)

The request object contains all information about the HTTP request.

#### req.params

Contains properties mapped to named route parameters.

```javascript
app.get('/users/:userId/posts/:postId', (req, res) => {
  console.log(req.params.userId);  // Access userId
  console.log(req.params.postId);  // Access postId
});
```

#### req.query

Holds a parsed query string as an object.

```javascript
// URL: /search?keyword=nodejs&page=2&limit=10
app.get('/search', (req, res) => {
  console.log(req.query.keyword);  // "nodejs"
  console.log(req.query.page);     // "2"
  console.log(req.query.limit);    // "10"
});
```

#### req.body

Contains the body of the request, valid for POST, PUT, and PATCH requests.

**Note:** req.body is undefined unless a middleware is installed to parse the body.

```javascript
// Enable JSON body parsing
app.use(express.json());

app.post('/users', (req, res) => {
  console.log(req.body.name);
  console.log(req.body.email);
});
```

#### req.headers / req.get(header)

Access request headers.

```javascript
app.get('/info', (req, res) => {
  console.log(req.headers);  // All headers as object
  console.log(req.get('Content-Type'));  // Specific header
  console.log(req.get('Authorization'));
});
```

#### req.path

Contains the path part of the URL (everything up to ?).

```javascript
// URL: /api/users?page=1
app.get('/api/users', (req, res) => {
  console.log(req.path);  // "/api/users"
});
```

#### req.url / req.originalUrl

Contains the complete URL including the query string.

```javascript
app.get('/api/users', (req, res) => {
  console.log(req.url);          // "/api/users?page=1"
  console.log(req.originalUrl);  // "/api/users?page=1"
});
```

### Request Object Properties Summary

| Property | Description | Example |
|----------|-------------|---------|
| req.params | Route parameters | `{ userId: "123" }` |
| req.query | Query string parameters | `{ page: "2", limit: "10" }` |
| req.body | Request body (POST/PUT/PATCH) | `{ name: "John" }` |
| req.headers | All request headers | `{ "content-type": "application/json" }` |
| req.get(header) | Specific header value | `"application/json"` |
| req.path | URL path without query | `"/api/users"` |
| req.url | Complete URL with query | `"/api/users?page=1"` |

### Response Object (res)

The response object is used to construct and send a response to the client.

#### res.send(body)

Sends a response with various types of content.

```javascript
// Send string
res.send('Hello World');

// Send object (automatically converted to JSON)
res.send({ message: 'Success' });

// Send array
res.send([1, 2, 3]);

// Send buffer
res.send(Buffer.from('Binary data'));
```

#### res.status(code)

Sets the response status code.

```javascript
// Success
res.status(200).send('OK');

// Created
res.status(201).send('Created');

// Bad Request
res.status(400).send('Bad Request');

// Unauthorized
res.status(401).send('Unauthorized');

// Not Found
res.status(404).send('Not Found');

// Server Error
res.status(500).send('Internal Server Error');
```

#### res.json(object)

Explicitly sends a JSON response.

```javascript
res.json({ 
  success: true, 
  data: { id: 1, name: 'John' } 
});
```

**Difference from res.send():**
- res.json() forces conversion to JSON
- res.send() may treat some parameters like null differently
- res.json() makes code more readable and explicit

#### res.sendFile(path)

Responds with the contents of a file.

```javascript
const path = require('path');

app.get('/download', (req, res) => {
  res.sendFile(path.join(__dirname, 'files', 'document.pdf'));
});
```

The content type is guessed using the file extension.

### Common HTTP Status Codes

| Code | Status | Meaning |
|------|--------|---------|
| 200 | OK | Request succeeded |
| 201 | Created | Resource created successfully |
| 204 | No Content | Success but no content to return |
| 400 | Bad Request | Invalid request data |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | Access denied |
| 404 | Not Found | Resource not found |
| 500 | Internal Server Error | Server error |

### Method Chaining

Response methods can be chained for cleaner code.

```javascript
res.status(201).json({ message: 'Created' });

res.status(404).send('Not Found');

res.status(500).json({ error: 'Server Error' });
```

### Send vs JSON

```javascript
// res.send() - auto-detects content type
app.get('/send', (req, res) => {
  res.send({ name: 'John' });  // Sends as JSON
  res.send('<h1>Hello</h1>');  // Sends as HTML
  res.send('Plain text');       // Sends as text
});

// res.json() - always sends as JSON
app.get('/json', (req, res) => {
  res.json({ name: 'John' });  // Always JSON
});
```

## 7. Middleware in Express

### What is Middleware?

An Express application is essentially a series of middleware function calls. Middleware functions are those that have access to:
- The request object (req)
- The response object (res)
- The next middleware function (next)

**Middleware can:**
- Execute any code
- Modify request and response objects
- End the request-response cycle
- Call the next middleware in the stack

### Middleware Function Structure

```javascript
function middlewareFunction(req, res, next) {
  // Execute code
  console.log('Middleware executed');
  
  // Modify request object
  req.customProperty = 'value';
  
  // Call next middleware
  next();
}
```

### Types of Middleware

#### 1. Built-in Middleware

Express provides built-in middleware functions.

**express.static:**
```javascript
// Serve static files
app.use(express.static('public'));
```

**express.json:**
```javascript
// Parse JSON bodies
app.use(express.json());
```

**express.urlencoded:**
```javascript
// Parse URL-encoded bodies
app.use(express.urlencoded({ extended: true }));
```

#### 2. Custom Middleware

You can create your own middleware functions.

**Logger Middleware Example:**
```javascript
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
  next();  // Pass control to next middleware
};

app.use(logger);
```

**Request Timestamp Middleware:**
```javascript
const addTimestamp = (req, res, next) => {
  req.timestamp = Date.now();
  next();
};

app.use(addTimestamp);
```

#### 3. Third-party Middleware

Install and use middleware from npm.

**Examples:**

**cors (Cross-Origin Resource Sharing):**
```javascript
const cors = require('cors');
app.use(cors());
```

**morgan (HTTP request logger):**
```javascript
const morgan = require('morgan');
app.use(morgan('combined'));
```

**helmet (Security headers):**
```javascript
const helmet = require('helmet');
app.use(helmet());
```

### Middleware Execution Order

Middleware is executed in the order it is defined.

```javascript
// First middleware
app.use((req, res, next) => {
  console.log('1st middleware');
  next();
});

// Second middleware
app.use((req, res, next) => {
  console.log('2nd middleware');
  next();
});

// Route handler
app.get('/', (req, res) => {
  console.log('Route handler');
  res.send('Response');
});
```

**Output:**
```
1st middleware
2nd middleware
Route handler
```

### Application-level vs Route-level Middleware

**Application-level (applies to all requests):**
```javascript
app.use(middlewareFunction);
```

**Path-specific (applies to specific paths):**
```javascript
app.use('/api', middlewareFunction);
```

**Route-specific:**
```javascript
app.get('/protected', authMiddleware, (req, res) => {
  res.send('Protected route');
});
```

### The next() Function

The next() function passes control to the next middleware in the stack.

**Rules:**
- Call next() to continue to next middleware
- Don't call next() if you send a response
- Call next(error) to skip to error handling middleware

```javascript
app.use((req, res, next) => {
  if (req.query.token) {
    next();  // Continue to next middleware
  } else {
    res.status(401).send('Unauthorized');  // End here
  }
});
```

### Middleware Pipeline Flow

```mermaid
graph LR
    A[Client Request] --> B[Middleware 1]
    B --> C[Middleware 2]
    C --> D[Middleware 3]
    D --> E[Route Handler]
    E --> F[Response to Client]
```

### Authentication Middleware Example

```javascript
const authenticate = (req, res, next) => {
  const token = req.headers['authorization'];
  
  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }
  
  // Verify token (simplified)
  if (token === 'valid-token') {
    req.user = { id: 1, name: 'John Doe' };
    next();  // Authenticated, proceed
  } else {
    res.status(403).json({ error: 'Invalid token' });
  }
};

// Use in route
app.get('/profile', authenticate, (req, res) => {
  res.json({ user: req.user });
});
```

### Logging Middleware Example

```javascript
const requestLogger = (req, res, next) => {
  const start = Date.now();
  
  // Log request
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
  
  // Log response when finished
  res.on('finish', () => {
    const duration = Date.now() - start;
    console.log(`Response sent in ${duration}ms - Status: ${res.statusCode}`);
  });
  
  next();
};

app.use(requestLogger);
```

### Error Handling Middleware

Error handling middleware has four arguments: (err, req, res, next)

```javascript
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({
    error: 'Something went wrong!',
    message: err.message
  });
});
```

## 8. REST APIs with MongoDB

### Connecting Express with MongoDB

Express applications connect to MongoDB using the MongoDB driver or Mongoose ODM.

**Using MongoDB Driver:**
```javascript
const { MongoClient } = require('mongodb');

const uri = 'mongodb://localhost:27017';
const client = new MongoClient(uri);

async function connectDB() {
  await client.connect();
  console.log('Connected to MongoDB');
  return client.db('mydatabase');
}
```

**Using Mongoose:**
```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/mydatabase', {
  useNewUrlParser: true,
  useUnifiedTopology: true
});

const db = mongoose.connection;
db.on('error', console.error.bind(console, 'Connection error:'));
db.once('open', () => {
  console.log('Connected to MongoDB');
});
```

### API-Database Interaction Flow

```mermaid
graph TD
    A[Client] -->|HTTP Request| B[Express Route]
    B --> C[Controller Function]
    C --> D[Database Query]
    D --> E[MongoDB]
    E -->|Query Result| D
    D --> C
    C --> F[Format Response]
    F -->|HTTP Response| A
```

### Data Flow in REST APIs

1. **Client sends HTTP request** to Express server
2. **Express router** matches the request to a route
3. **Middleware** processes the request (auth, validation, etc.)
4. **Route handler** executes business logic
5. **Database operation** performed on MongoDB
6. **Data retrieved/modified** in MongoDB
7. **Response formatted** (usually as JSON)
8. **HTTP response sent** back to client

**Complete Flow Example:**
```javascript
// 1. Client Request: POST /api/users
// Body: { name: "John", email: "john@example.com" }

// 2. Express receives request
app.post('/api/users', async (req, res) => {
  try {
    // 3. Extract data from request
    const { name, email } = req.body;
    
    // 4. Create database document
    const user = new User({ name, email });
    
    // 5. Save to MongoDB
    await user.save();
    
    // 6. Send response
    res.status(201).json({ 
      message: 'User created',
      data: user 
    });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

## 9. The LIST API

### Purpose of LIST API

The LIST API is used to retrieve a collection of resources from the database. It typically supports:
- Retrieving all records
- Filtering based on criteria
- Sorting
- Pagination
- Field selection

### REST Endpoint Design

**Standard LIST endpoint:**
```
GET /api/resources
```

**Examples:**
```
GET /api/users
GET /api/products
GET /api/customers
```

**With Query Parameters:**
```
GET /api/users?page=1&limit=10
GET /api/products?category=electronics&sort=price
GET /api/customers?city=Bangalore&status=active
```

### Controller Logic

The controller handles the business logic for the LIST operation.

```javascript
const listUsers = async (req, res) => {
  try {
    // Extract query parameters
    const { page = 1, limit = 10, sort = 'createdAt' } = req.query;
    
    // Calculate skip value for pagination
    const skip = (page - 1) * limit;
    
    // Fetch users from database
    const users = await User.find()
      .sort(sort)
      .skip(skip)
      .limit(parseInt(limit));
    
    // Get total count
    const total = await User.countDocuments();
    
    // Send response
    res.json({
      success: true,
      data: users,
      pagination: {
        page: parseInt(page),
        limit: parseInt(limit),
        total: total,
        pages: Math.ceil(total / limit)
      }
    });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};
```

### Database Interaction

**Basic Query:**
```javascript
const users = await User.find();
```

**With Filtering:**
```javascript
const users = await User.find({ city: 'Bangalore', status: 'active' });
```

**With Sorting:**
```javascript
const users = await User.find().sort({ createdAt: -1 });  // Descending
```

**With Pagination:**
```javascript
const users = await User.find()
  .skip(10)   // Skip first 10 records
  .limit(10); // Return next 10 records
```

**With Field Selection:**
```javascript
const users = await User.find().select('name email');  // Only return name and email
```

### Response Structure

**Successful Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "507f1f77bcf86cd799439011",
      "name": "John Doe",
      "email": "john@example.com",
      "createdAt": "2025-12-20T10:30:00Z"
    },
    {
      "_id": "507f1f77bcf86cd799439012",
      "name": "Jane Smith",
      "email": "jane@example.com",
      "createdAt": "2025-12-21T09:15:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 25,
    "pages": 3
  }
}
```

**Empty Result Response:**
```json
{
  "success": true,
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 0,
    "pages": 0
  }
}
```

### Complete LIST API Example

```javascript
// Route definition
app.get('/api/products', listProducts);

// Controller function
async function listProducts(req, res) {
  try {
    // Extract query parameters
    const { 
      category, 
      minPrice, 
      maxPrice, 
      page = 1, 
      limit = 10,
      sort = '-createdAt' 
    } = req.query;
    
    // Build filter object
    const filter = {};
    if (category) filter.category = category;
    if (minPrice) filter.price = { $gte: parseFloat(minPrice) };
    if (maxPrice) filter.price = { ...filter.price, $lte: parseFloat(maxPrice) };
    
    // Calculate pagination
    const skip = (page - 1) * limit;
    
    // Query database
    const products = await Product.find(filter)
      .sort(sort)
      .skip(skip)
      .limit(parseInt(limit));
    
    // Get total count
    const total = await Product.countDocuments(filter);
    
    // Send response
    res.status(200).json({
      success: true,
      count: products.length,
      data: products,
      pagination: {
        page: parseInt(page),
        limit: parseInt(limit),
        total: total,
        pages: Math.ceil(total / limit)
      }
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
}
```

## 10. The CREATE API

### Purpose of CREATE API

The CREATE API is used to add a new resource to the database. It:
- Accepts data in request body
- Validates the data
- Creates new record in database
- Returns the created resource

### POST Request Handling

CREATE operations use the POST HTTP method.

**Endpoint Design:**
```
POST /api/resources
```

**Examples:**
```
POST /api/users
POST /api/products
POST /api/orders
```

### Validation

Validation ensures data integrity before saving to database.

**Manual Validation:**
```javascript
function validateUser(data) {
  const errors = [];
  
  if (!data.name || data.name.trim() === '') {
    errors.push('Name is required');
  }
  
  if (!data.email || !data.email.includes('@')) {
    errors.push('Valid email is required');
  }
  
  if (!data.age || data.age < 18) {
    errors.push('Age must be 18 or above');
  }
  
  return errors;
}
```

**Using Mongoose Schema Validation:**
```javascript
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Name is required'],
    trim: true,
    minlength: [3, 'Name must be at least 3 characters']
  },
  email: {
    type: String,
    required: [true, 'Email is required'],
    unique: true,
    lowercase: true,
    match: [/^\S+@\S+\.\S+$/, 'Please provide valid email']
  },
  age: {
    type: Number,
    min: [18, 'Age must be at least 18']
  }
});
```

### Database Insert Logic

**Using Mongoose:**
```javascript
// Create new document
const user = new User({
  name: req.body.name,
  email: req.body.email,
  age: req.body.age
});

// Save to database
await user.save();
```

**Using MongoDB Driver:**
```javascript
const result = await db.collection('users').insertOne({
  name: req.body.name,
  email: req.body.email,
  age: req.body.age,
  createdAt: new Date()
});
```

### Response Handling

**Success Response (201 Created):**
```json
{
  "success": true,
  "message": "User created successfully",
  "data": {
    "_id": "507f1f77bcf86cd799439011",
    "name": "John Doe",
    "email": "john@example.com",
    "age": 25,
    "createdAt": "2025-12-21T10:30:00Z"
  }
}
```

**Validation Error Response (400 Bad Request):**
```json
{
  "success": false,
  "errors": [
    "Name is required",
    "Valid email is required"
  ]
}
```

**Duplicate Entry Error (409 Conflict):**
```json
{
  "success": false,
  "error": "Email already exists"
}
```

### Complete CREATE API Example

```javascript
// Route definition
app.post('/api/users', createUser);

// Controller function
async function createUser(req, res) {
  try {
    // Extract data from request body
    const { name, email, age, city } = req.body;
    
    // Validate input
    if (!name || !email) {
      return res.status(400).json({
        success: false,
        error: 'Name and email are required'
      });
    }
    
    // Check if email already exists
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return res.status(409).json({
        success: false,
        error: 'Email already exists'
      });
    }
    
    // Create new user
    const user = new User({
      name,
      email,
      age,
      city
    });
    
    // Save to database
    await user.save();
    
    // Send success response
    res.status(201).json({
      success: true,
      message: 'User created successfully',
      data: user
    });
    
  } catch (error) {
    // Handle validation errors
    if (error.name === 'ValidationError') {
      const errors = Object.values(error.errors).map(err => err.message);
      return res.status(400).json({
        success: false,
        errors: errors
      });
    }
    
    // Handle other errors
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
}
```

## 11. Using the LIST API

### Client Request Example

**Using Fetch API (JavaScript):**
```javascript
// Basic request
fetch('http://localhost:3000/api/users')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));

// With query parameters
const params = new URLSearchParams({
  page: 1,
  limit: 10,
  city: 'Bangalore'
});

fetch(`http://localhost:3000/api/users?${params}`)
  .then(response => response.json())
  .then(data => {
    console.log('Users:', data.data);
    console.log('Total:', data.pagination.total);
  });
```

**Using Axios:**
```javascript
const axios = require('axios');

// Basic request
axios.get('http://localhost:3000/api/users')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('Error:', error.message);
  });

// With parameters
axios.get('http://localhost:3000/api/users', {
  params: {
    page: 1,
    limit: 10,
    city: 'Bangalore',
    sort: '-createdAt'
  }
})
  .then(response => {
    const { data, pagination } = response.data;
    console.log(`Showing ${data.length} of ${pagination.total} users`);
  });
```

### Server Processing Flow

1. **Request Received**: Express receives GET request at /api/users
2. **Route Matching**: Router matches request to appropriate handler
3. **Middleware Execution**: Authentication, logging, etc.
4. **Query Parameters Extraction**: page, limit, filters extracted from req.query
5. **Database Query**: MongoDB query executed with filters and pagination
6. **Data Retrieval**: Records fetched from database
7. **Response Formatting**: Data formatted into JSON structure
8. **Response Sent**: JSON response sent to client with status 200

```javascript
// Server-side flow
app.get('/api/users', async (req, res) => {
  // Step 1: Extract parameters
  const { page, limit, city } = req.query;
  
  // Step 2: Build query
  const filter = city ? { city } : {};
  const skip = (page - 1) * limit;
  
  // Step 3: Execute database query
  const users = await User.find(filter)
    .skip(skip)
    .limit(parseInt(limit));
  
  // Step 4: Get total count
  const total = await User.countDocuments(filter);
  
  // Step 5: Format and send response
  res.json({
    success: true,
    data: users,
    pagination: { page, limit, total }
  });
});
```

### JSON Response Example

**Request:**
```
GET http://localhost:3000/api/products?category=electronics&page=1&limit=5
```

**Response:**
```json
{
  "success": true,
  "count": 5,
  "data": [
    {
      "_id": "507f1f77bcf86cd799439011",
      "name": "Laptop",
      "category": "electronics",
      "price": 50000,
      "quantity": 10,
      "createdAt": "2025-12-20T10:30:00Z"
    },
    {
      "_id": "507f1f77bcf86cd799439012",
      "name": "Smartphone",
      "category": "electronics",
      "price": 25000,
      "quantity": 25,
      "createdAt": "2025-12-21T09:15:00Z"
    },
    {
      "_id": "507f1f77bcf86cd799439013",
      "name": "Tablet",
      "category": "electronics",
      "price": 30000,
      "quantity": 15,
      "createdAt": "2025-12-21T11:20:00Z"
    },
    {
      "_id": "507f1f77bcf86cd799439014",
      "name": "Headphones",
      "category": "electronics",
      "price": 2000,
      "quantity": 50,
      "createdAt": "2025-12-21T14:45:00Z"
    },
    {
      "_id": "507f1f77bcf86cd799439015",
      "name": "Smartwatch",
      "category": "electronics",
      "price": 15000,
      "quantity": 20,
      "createdAt": "2025-12-21T16:10:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 5,
    "total": 23,
    "pages": 5
  }
}
```

## 12. Using the CREATE API

### Client Request Payload

**Using Fetch API:**
```javascript
const newUser = {
  name: 'John Doe',
  email: 'john@example.com',
  age: 28,
  city: 'Bangalore'
};

fetch('http://localhost:3000/api/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(newUser)
})
  .then(response => response.json())
  .then(data => {
    console.log('Success:', data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**Using Axios:**
```javascript
const axios = require('axios');

const newProduct = {
  name: 'Laptop',
  category: 'electronics',
  price: 50000,
  quantity: 10
};

axios.post('http://localhost:3000/api/products', newProduct)
  .then(response => {
    console.log('Product created:', response.data);
  })
  .catch(error => {
    if (error.response) {
      console.error('Error:', error.response.data);
    }
  });
```

### Server-side Processing

**Complete Flow:**

1. **Request Received**: POST request received at /api/users
2. **Body Parsing**: express.json() middleware parses request body
3. **Validation**: Data validated against rules
4. **Duplicate Check**: Check if resource already exists
5. **Document Creation**: New document created in memory
6. **Database Save**: Document saved to MongoDB
7. **Response Generation**: Success response with created resource
8. **Response Sent**: JSON response with status 201

```javascript
app.post('/api/users', async (req, res) => {
  try {
    // Step 1: Extract data
    const { name, email, age } = req.body;
    
    // Step 2: Validate
    if (!name || !email) {
      return res.status(400).json({
        success: false,
        error: 'Name and email required'
      });
    }
    
    // Step 3: Check duplicates
    const exists = await User.findOne({ email });
    if (exists) {
      return res.status(409).json({
        success: false,
        error: 'Email already exists'
      });
    }
    
    // Step 4: Create and save
    const user = new User({ name, email, age });
    await user.save();
    
    // Step 5: Send response
    res.status(201).json({
      success: true,
      data: user
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
});
```

### Error Cases

#### 1. Missing Required Fields

**Request:**
```json
POST /api/users
{
  "email": "john@example.com"
}
```

**Response (400 Bad Request):**
```json
{
  "success": false,
  "error": "Name is required"
}
```

#### 2. Invalid Data Format

**Request:**
```json
POST /api/users
{
  "name": "John",
  "email": "invalid-email",
  "age": "not-a-number"
}
```

**Response (400 Bad Request):**
```json
{
  "success": false,
  "errors": [
    "Please provide valid email",
    "Age must be a number"
  ]
}
```

#### 3. Duplicate Entry

**Request:**
```json
POST /api/users
{
  "name": "John Doe",
  "email": "existing@example.com"
}
```

**Response (409 Conflict):**
```json
{
  "success": false,
  "error": "Email already exists"
}
```

#### 4. Server Error

**Response (500 Internal Server Error):**
```json
{
  "success": false,
  "error": "Database connection failed"
}
```

### Success Response

**Request:**
```json
POST /api/users
{
  "name": "John Doe",
  "email": "john@example.com",
  "age": 28,
  "city": "Bangalore"
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "message": "User created successfully",
  "data": {
    "_id": "507f1f77bcf86cd799439011",
    "name": "John Doe",
    "email": "john@example.com",
    "age": 28,
    "city": "Bangalore",
    "createdAt": "2025-12-21T10:30:00Z",
    "updatedAt": "2025-12-21T10:30:00Z"
  }
}
```

## 13. Error Handling in REST APIs

### Types of Errors

1. **Client Errors (4xx)**: Errors caused by invalid client requests
2. **Server Errors (5xx)**: Errors caused by server issues
3. **Validation Errors**: Invalid data format or missing required fields
4. **Database Errors**: Connection issues, query failures
5. **Authentication Errors**: Invalid or missing credentials
6. **Authorization Errors**: Insufficient permissions

### HTTP Status Codes

#### Success Codes (2xx)

| Code | Status | Usage |
|------|--------|-------|
| 200 | OK | Successful GET, PUT, PATCH, DELETE |
| 201 | Created | Successful POST (resource created) |
| 204 | No Content | Successful DELETE (no content returned) |

#### Client Error Codes (4xx)

| Code | Status | Usage |
|------|--------|-------|
| 400 | Bad Request | Invalid request data or format |
| 401 | Unauthorized | Authentication required or failed |
| 403 | Forbidden | Authenticated but no permission |
| 404 | Not Found | Resource does not exist |
| 409 | Conflict | Resource already exists (duplicate) |
| 422 | Unprocessable Entity | Validation errors |

#### Server Error Codes (5xx)

| Code | Status | Usage |
|------|--------|-------|
| 500 | Internal Server Error | Generic server error |
| 502 | Bad Gateway | Invalid response from upstream |
| 503 | Service Unavailable | Server temporarily unavailable |
| 504 | Gateway Timeout | Upstream server timeout |

### Centralized Error Handling

**Custom Error Class:**
```javascript
class APIError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = true;
    Error.captureStackTrace(this, this.constructor);
  }
}

module.exports = APIError;
```

**Using Custom Error:**
```javascript
const APIError = require('./APIError');

app.get('/api/users/:id', async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    
    if (!user) {
      throw new APIError('User not found', 404);
    }
    
    res.json({ success: true, data: user });
  } catch (error) {
    next(error);  // Pass to error handler
  }
});
```

### Error Middleware

Error handling middleware must have four parameters: (err, req, res, next)

**Basic Error Handler:**
```javascript
app.use((err, req, res, next) => {
  console.error(err.stack);
  
  res.status(err.statusCode || 500).json({
    success: false,
    error: err.message || 'Internal Server Error'
  });
});
```

**Comprehensive Error Handler:**
```javascript
app.use((err, req, res, next) => {
  // Set default values
  err.statusCode = err.statusCode || 500;
  err.message = err.message || 'Internal Server Error';
  
  // Log error
  console.error(`[ERROR] ${err.statusCode} - ${err.message}`);
  console.error(err.stack);
  
  // Mongoose validation error
  if (err.name === 'ValidationError') {
    const errors = Object.values(err.errors).map(e => e.message);
    return res.status(400).json({
      success: false,
      errors: errors
    });
  }
  
  // Mongoose duplicate key error
  if (err.code === 11000) {
    const field = Object.keys(err.keyPattern)[0];
    return res.status(409).json({
      success: false,
      error: `${field} already exists`
    });
  }
  
  // Mongoose cast error (invalid ID)
  if (err.name === 'CastError') {
    return res.status(400).json({
      success: false,
      error: 'Invalid ID format'
    });
  }
  
  // JWT errors
  if (err.name === 'JsonWebTokenError') {
    return res.status(401).json({
      success: false,
      error: 'Invalid token'
    });
  }
  
  if (err.name === 'TokenExpiredError') {
    return res.status(401).json({
      success: false,
      error: 'Token expired'
    });
  }
  
  // Default error response
  res.status(err.statusCode).json({
    success: false,
    error: err.message,
    ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
  });
});
```

### Best Practices for API Error Responses

#### 1. Consistent Error Format

Use a consistent structure for all error responses:

```json
{
  "success": false,
  "error": "Error message here",
  "code": "ERROR_CODE",
  "details": {}
}
```

#### 2. Descriptive Error Messages

```javascript
// Bad
res.status(400).json({ error: 'Invalid' });

// Good
res.status(400).json({ 
  error: 'Invalid email format. Please provide a valid email address.' 
});
```

#### 3. Include Error Codes

```javascript
res.status(404).json({
  success: false,
  error: 'User not found',
  code: 'USER_NOT_FOUND'
});
```

#### 4. Validation Error Details

```javascript
res.status(400).json({
  success: false,
  error: 'Validation failed',
  errors: [
    { field: 'email', message: 'Email is required' },
    { field: 'age', message: 'Age must be at least 18' }
  ]
});
```

#### 5. Don't Expose Sensitive Information

```javascript
// Bad - exposes internal details
res.status(500).json({
  error: 'MongoError: Connection to mongodb://admin:pass123@localhost failed'
});

// Good - generic message
res.status(500).json({
  error: 'Database connection failed'
});
```

### Async Error Handling Helper

**Wrapper Function:**
```javascript
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Usage
app.get('/api/users/:id', asyncHandler(async (req, res) => {
  const user = await User.findById(req.params.id);
  
  if (!user) {
    throw new APIError('User not found', 404);
  }
  
  res.json({ success: true, data: user });
}));
```

### Complete Error Handling Example

```javascript
const express = require('express');
const app = express();

// Custom error class
class APIError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
  }
}

// Async handler
const asyncHandler = fn => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Routes
app.get('/api/users/:id', asyncHandler(async (req, res) => {
  const user = await User.findById(req.params.id);
  
  if (!user) {
    throw new APIError('User not found', 404);
  }
  
  res.json({ success: true, data: user });
}));

// 404 handler (must be after all routes)
app.use((req, res, next) => {
  res.status(404).json({
    success: false,
    error: 'Route not found'
  });
});

// Global error handler (must be last)
app.use((err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  
  res.status(err.statusCode).json({
    success: false,
    error: err.message,
    ...(process.env.NODE_ENV === 'development' && { 
      stack: err.stack 
    })
  });
});

app.listen(3000);
```

## 14. GraphQL

### Introduction to GraphQL

GraphQL is a query language for your API, and a server-side runtime for executing queries using a type system you define for your data.

**Key Advantages:**
- Client specifies exactly what data it needs
- Single endpoint for all operations
- Strongly typed schema
- No over-fetching or under-fetching of data
- Real-time updates with subscriptions

### Key Components of GraphQL

#### 1. Schema

Defines the structure of data and operations available.

```javascript
const typeDefs = `
  type Book {
    id: ID!
    title: String!
    author: String!
    price: Float!
    quantity: Int!
  }
  
  type Query {
    books: [Book]
    book(id: ID!): Book
  }
  
  type Mutation {
    addBook(title: String!, author: String!, price: Float!, quantity: Int!): Book
    deleteBook(id: ID!): String
    updateBook(id: ID!, title: String, author: String, price: Float, quantity: Int): Book
  }
`;

// Resolvers
const resolvers = {
  Query: {
    books: () => books,
    book: (parent, args) => books.find(book => book.id === parseInt(args.id))
  },
  Mutation: {
    addBook: (parent, args) => {
      const newBook = {
        id: books.length + 1,
        title: args.title,
        author: args.author,
        price: args.price,
        quantity: args.quantity
      };
      books.push(newBook);
      return newBook;
    },
    deleteBook: (parent, args) => {
      const initialLength = books.length;
      books = books.filter(book => book.id !== parseInt(args.id));
      return books.length === initialLength ? 'Book not found' : 'Book deleted successfully';
    },
    updateBook: (parent, args) => {
      const book = books.find(book => book.id === parseInt(args.id));
      if (!book) return null;
      if (args.title !== undefined) book.title = args.title;
      if (args.author !== undefined) book.author = args.author;
      if (args.price !== undefined) book.price = args.price;
      if (args.quantity !== undefined) book.quantity = args.quantity;
      return book;
    }
  }
};

// Start server function
async function startServer() {
  const apolloServer = new ApolloServer({
    typeDefs,
    resolvers
  });
  
  await apolloServer.start();
  
  // Apply Apollo GraphQL middleware to Express app
  app.use(
    '/graphql',  // GraphQL endpoint
    express.json(),
    cors(),
    expressMiddleware(apolloServer)
  );
  
  app.use(express.json());
  
  app.listen(port, () => {
    console.log(`Server running on http://localhost:${port}`);
    console.log(`GraphQL endpoint: http://localhost:${port}/graphql`);
  });
}

startServer();
```

**Testing Endpoint:**
Use Apollo Studio Sandbox: `https://studio.apollographql.com/sandbox/explorer`

### GraphQL API Integration with React JS

**Setup:**
```bash
npm install axios
```

**React Component with GraphQL:**

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';

const GRAPHQL_ENDPOINT = 'http://localhost:3000/graphql';

function BookManager() {
  const [books, setBooks] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [newBook, setNewBook] = useState({
    title: '',
    author: '',
    price: 0,
    quantity: 0
  });

  // Fetch books (Query operation)
  const fetchBooks = async () => {
    setLoading(true);
    
    const GET_BOOKS_QUERY = `
      query {
        books {
          id
          title
          author
          price
          quantity
        }
      }
    `;
    
    try {
      const response = await axios.post(GRAPHQL_ENDPOINT, {
        query: GET_BOOKS_QUERY
      });
      
      if (response.data.errors) {
        throw new Error(response.data.errors[0].message);
      }
      
      setBooks(response.data.data.books);
    } catch (err) {
      setError(err.message || 'Failed to fetch books');
      console.error('Fetch Books Error:', err);
    } finally {
      setLoading(false);
    }
  };
  
  // Add book (Mutation operation)
  const addBook = async (e) => {
    e.preventDefault();
    
    if (!newBook.title || !newBook.author) return;
    
    const ADD_BOOK_MUTATION = `
      mutation AddBook($title: String!, $author: String!, $price: Float!, $quantity: Int!) {
        addBook(title: $title, author: $author, price: $price, quantity: $quantity) {
          id
          title
          author
          price
          quantity
        }
      }
    `;
    
    try {
      const response = await axios.post(GRAPHQL_ENDPOINT, {
        query: ADD_BOOK_MUTATION,
        variables: {
          title: newBook.title,
          author: newBook.author,
          price: parseFloat(newBook.price),
          quantity: parseInt(newBook.quantity)
        }
      });
      
      if (response.data.errors) {
        throw new Error(response.data.errors[0].message);
      }
      
      // Refetch books to update list
      fetchBooks();
      
      // Reset form
      setNewBook({ title: '', author: '', price: 0, quantity: 0 });
    } catch (err) {
      setError(err.message || 'Failed to add book');
      console.error('Add Book Error:', err);
    }
  };
  
  useEffect(() => {
    fetchBooks();
  }, []);
  
  return (
    <div>
      <h1>Book Manager</h1>
      {loading && <p>Loading...</p>}
      {error && <p>Error: {error}</p>}
      
      <form onSubmit={addBook}>
        <input 
          value={newBook.title}
          onChange={(e) => setNewBook({...newBook, title: e.target.value})}
          placeholder="Title"
        />
        <input 
          value={newBook.author}
          onChange={(e) => setNewBook({...newBook, author: e.target.value})}
          placeholder="Author"
        />
        <button type="submit">Add Book</button>
      </form>
      
      <ul>
        {books.map(book => (
          <li key={book.id}>
            {book.title} by {book.author} - Rs.{book.price}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default BookManager;
```

### GraphQL Error Handling

**Server-side Error Handling:**

```javascript
const resolvers = {
  Query: {
    book: (parent, args) => {
      const book = books.find(b => b.id === parseInt(args.id));
      if (!book) {
        throw new Error('Book not found');
      }
      return book;
    }
  },
  
  Mutation: {
    addBook: (parent, args) => {
      // Validation
      if (!args.title || args.title.trim() === '') {
        throw new Error('Title is required');
      }
      
      if (args.price < 0) {
        throw new Error('Price cannot be negative');
      }
      
      const newBook = {
        id: books.length + 1,
        title: args.title,
        author: args.author,
        price: args.price,
        quantity: args.quantity
      };
      
      books.push(newBook);
      return newBook;
    }
  }
};
```

**Client-side Error Handling:**

```javascript
try {
  const response = await axios.post(GRAPHQL_ENDPOINT, {
    query: GET_BOOKS_QUERY
  });
  
  // Check for GraphQL errors
  if (response.data.errors) {
    throw new Error(response.data.errors[0].message);
  }
  
  setBooks(response.data.data.books);
  
} catch (err) {
  setError(err.message || 'Failed to fetch books');
  console.error('Fetch Books Error:', err);
} finally {
  setLoading(false);
}
```

### GraphQL vs REST Comparison

| Feature | REST | GraphQL |
|---------|------|---------|
| Endpoints | Multiple endpoints | Single endpoint |
| Data Fetching | Fixed structure | Client specifies fields |
| Over-fetching | Common | Eliminated |
| Under-fetching | Requires multiple requests | Single request |
| Versioning | URL versioning | Schema evolution |
| Caching | HTTP caching | Requires custom solution |
| Learning Curve | Easier | Steeper |
| Tooling | Mature | Growing |

---

## Important Diagrams

### REST Request-Response Lifecycle

```mermaid
sequenceDiagram
    participant Client
    participant Express
    participant Middleware
    participant Route Handler
    participant Database
    
    Client->>Express: HTTP Request
    Express->>Middleware: Process Request
    Middleware->>Middleware: Authentication
    Middleware->>Middleware: Logging
    Middleware->>Route Handler: next()
    Route Handler->>Database: Query
    Database-->>Route Handler: Data
    Route Handler->>Express: Format Response
    Express-->>Client: HTTP Response
```

### Express Middleware Pipeline

```mermaid
graph LR
    A[Request] --> B[express.json]
    B --> C[express.static]
    C --> D[Custom Logger]
    D --> E[Authentication]
    E --> F[Route Handler]
    F --> G[Response]
```

### REST API Data Flow with MongoDB

```mermaid
graph TD
    A[Client] -->|HTTP Request| B[Express Server]
    B --> C[Middleware Layer]
    C --> D{Route Matcher}
    D -->|Match| E[Controller]
    D -->|No Match| F[404 Handler]
    E --> G[Mongoose Model]
    G --> H[MongoDB]
    H -->|Query Result| G
    G --> E
    E --> I[Format JSON]
    I -->|HTTP Response| A
    F -->|404 Response| A
```

### CREATE API Flow

```mermaid
flowchart TD
    A[Client POST Request] --> B[Express receives request]
    B --> C[express.json parses body]
    C --> D{Validate Data}
    D -->|Invalid| E[Return 400 Error]
    D -->|Valid| F{Check Duplicates}
    F -->|Exists| G[Return 409 Conflict]
    F -->|Not Exists| H[Create Document]
    H --> I[Save to MongoDB]
    I -->|Success| J[Return 201 Created]
    I -->|Failure| K[Return 500 Error]
```

### GraphQL Architecture

```mermaid
graph TB
    A[Client] -->|Query/Mutation| B[GraphQL Endpoint]
    B --> C[Apollo Server]
    C --> D{Type Validation}
    D -->|Valid| E[Resolver Functions]
    D -->|Invalid| F[Return Error]
    E --> G[Data Source]
    G --> H[Database]
    H -->|Data| G
    G --> E
    E --> I[Format Response]
    I -->|JSON| A
```

---

## 20 Important Exam Questions for UNIT IV

### Short Answer Questions (2-5 marks)

1. What is REST? Explain the resource-based architecture of REST APIs.

2. Differentiate between stateless and stateful architecture with examples.

3. Explain the difference between PUT and PATCH HTTP methods with examples.

4. What is idempotency in REST APIs? Which HTTP methods are idempotent?

5. Compare JSON and XML as data interchange formats.

6. What is middleware in Express? List three types of middleware with examples.

7. Explain the purpose of the next() function in Express middleware.

8. What is the difference between req.params and req.query? Give examples.

9. What are the advantages of using Express framework for building REST APIs?

10. Explain the difference between res.send() and res.json() methods.

### Long Answer Questions (8-10 marks)

11. Explain the complete lifecycle of a REST API request in Express, from client request to server response. Include the role of middleware, routing, and database interaction.

12. Describe the LIST API in detail. Explain its purpose, endpoint design, controller logic, database interaction, and response structure with code examples.

13. Explain the CREATE API with complete implementation. Include validation, error handling, database insertion, and different response scenarios.

14. What is error handling in REST APIs? Explain different types of errors, HTTP status codes, and centralized error handling with code examples.

15. Explain the middleware execution order in Express. How does the middleware pipeline work? Provide authentication and logging middleware examples.

16. Compare REST and GraphQL architectures. Explain the advantages and disadvantages of each approach with examples.

### Code-Based Questions (10 marks)

17. Write a complete Express application that implements a REST API for managing products with the following features:
    - GET /api/products (List all products with pagination)
    - POST /api/products (Create new product with validation)
    - PUT /api/products/:id (Update product)
    - DELETE /api/products/:id (Delete product)
    Include error handling middleware.

18. Implement a custom authentication middleware in Express that:
    - Checks for Authorization header
    - Validates the token
    - Attaches user information to request object
    - Returns 401 if unauthorized
    Show how to use this middleware in protected routes.

19. Create a GraphQL schema and resolvers for a Book Management System that supports:
    - Query to get all books
    - Query to get a single book by ID
    - Mutation to add a new book
    - Mutation to update book details
    - Mutation to delete a book
    Include error handling in resolvers.

### Diagram-Based Questions (5-8 marks)

20. Draw and explain the following diagrams:
    a) REST API request-response lifecycle showing client, Express server, middleware, route handler, and MongoDB
    b) Middleware execution pipeline in Express
    c) CREATE API flow diagram showing validation, duplicate checking, and database insertion
    d) GraphQL architecture showing query flow from client to database and back

---

## Revision Tips for Maximum Marks

1. **Understand REST Principles**: Know the difference between REST and other architectures. Focus on statelessness and resource-based design.

2. **Master HTTP Methods**: Memorize the CRUD mapping table. Know which methods are idempotent and safe.

3. **Express Fundamentals**: Understand req and res objects thoroughly. Know all their properties and methods.

4. **Middleware Concepts**: Practice writing custom middleware. Understand execution order and the next() function.

5. **Error Handling**: Remember all HTTP status codes. Practice writing centralized error handlers.

6. **Code Practice**: Write complete API implementations from scratch. Include validation and error handling.

7. **GraphQL Basics**: Understand schema, queries, mutations, and resolvers. Know how to integrate with React.

8. **Diagrams**: Practice drawing flow diagrams. They fetch easy marks in exams.

9. **Comparison Tables**: Memorize tables comparing HTTP methods, JSON vs XML, REST vs GraphQL.

10. **Real Examples**: Use practical examples in answers like e-commerce, user management, book store, etc.

---

## Quick Reference Tables

### HTTP Methods Quick Reference

| Method | Operation | Example | Body | Idempotent |
|--------|-----------|---------|------|------------|
| GET | Read | `/api/users` | No | Yes |
| POST | Create | `/api/users` | Yes | No |
| PUT | Replace | `/api/users/1` | Yes | Yes |
| PATCH | Update | `/api/users/1` | Yes | No |
| DELETE | Delete | `/api/users/1` | No | Yes |

### Express Request Object Properties

| Property | Description | Example |
|----------|-------------|---------|
| req.params | Route parameters | `{ id: "123" }` |
| req.query | Query string | `{ page: "1" }` |
| req.body | Request body | `{ name: "John" }` |
| req.headers | Request headers | `{ "content-type": "application/json" }` |

### HTTP Status Codes

| Code | Meaning | Usage |
|------|---------|-------|
| 200 | OK | Success |
| 201 | Created | Resource created |
| 400 | Bad Request | Invalid data |
| 401 | Unauthorized | Auth required |
| 404 | Not Found | Resource missing |
| 500 | Server Error | Server issue |

---

**End of UNIT IV Notes**: Int): Book
  }
`;
```

#### 2. Queries (for retrieving data)

Queries are used to retrieve data from the GraphQL server.

**Example Query:**
```graphql
query {
  books {
    id
    title
    author
    price
  }
}
```

**Query with Parameters:**
```graphql
query {
  book(id: "1") {
    title
    author
  }
}
```

#### 3. Mutations (for modifying data)

Mutations are used to modify data on the server (create, update, delete).

**Example Mutation:**
```graphql
mutation {
  addBook(
    title: "GraphQL Basics"
    author: "John Doe"
    price: 599.99
    quantity: 10
  ) {
    id
    title
    author
  }
}
```

#### 4. Types

##### Scalar Types

Represent primitive values:
- **Int**: Integer numbers
- **Float**: Decimal numbers
- **String**: Text data
- **Boolean**: true or false
- **ID**: Unique identifier

##### Object Types

Represent complex objects with fields.

```javascript
type User {
  id: ID!
  name: String!
  email: String!
  age: Int
  posts: [Post]
}

type Post {
  id: ID!
  title: String!
  content: String!
  author: User!
}
```

**Field Modifiers:**
- `!` - Non-nullable (required)
- `[]` - Array/List
- `[Type!]!` - Non-nullable array of non-nullable items

### GraphQL Schema Design Example

```javascript
type Book {
  id: ID!
  title: String!
  author: String!
  price: Float!
  quantity: Int!
}

type Query {
  books: [Book!]!
  book(id: ID!): Book
}

type Mutation {
  createBook(title: String!, author: String!, price: Float!, quantity: Int!): Book
  updateBook(id: ID!, title: String, author: String, price: Float, quantity: Int): Book
  deleteBook(id: ID!): Boolean
}
```

### GraphQL Resolvers

Resolvers are functions that handle the logic for queries and mutations.

**Installation:**
```bash
npm install @apollo/server graphql
```

**Complete Resolver Example:**

```javascript
const resolvers = {
  Query: {
    // Get all books
    books: () => books,
    
    // Get single book by ID
    book: (parent, args) => {
      return books.find(book => book.id === parseInt(args.id));
    }
  },
  
  Mutation: {
    // Add new book
    addBook: (parent, args) => {
      const newBook = {
        id: books.length + 1,
        title: args.title,
        author: args.author,
        price: args.price,
        quantity: args.quantity
      };
      books.push(newBook);
      return newBook;
    },
    
    // Delete book
    deleteBook: (parent, args) => {
      const initialLength = books.length;
      books = books.filter(book => book.id !== parseInt(args.id));
      
      if (books.length === initialLength) {
        return 'Book not found';
      } else {
        return 'Book deleted successfully';
      }
    },
    
    // Update book
    updateBook: (parent, args) => {
      const book = books.find(book => book.id === parseInt(args.id));
      if (!book) return null;
      
      if (args.title !== undefined) book.title = args.title;
      if (args.author !== undefined) book.author = args.author;
      if (args.price !== undefined) book.price = args.price;
      if (args.quantity !== undefined) book.quantity = args.quantity;
      
      return book;
    }
  }
};
```

### GraphQL Server Setup

```javascript
const express = require('express');
const cors = require('cors');
const { ApolloServer } = require('@apollo/server');
const { expressMiddleware } = require('@apollo/server/express4');

const app = express();
const port = 3000;

// Sample data
let books = [
  { id: 1, title: 'Book 1', author: 'Author 1', price: 299.99, quantity: 10 },
  { id: 2, title: 'Book 2', author: 'Author 2', price: 399.99, quantity: 5 }
];

// Type definitions
const typeDefs = `
  type Book {
    id: ID!
    title: String!
    author: String!
    price: Float!
    quantity: Int!
  }
  
  type Query {
    books: [Book]
    book(id: ID!): Book
  }
  
  type Mutation {
    addBook(title: String!, author: String!, price: Float!, quantity: Int!): Book
    deleteBook(id: ID!): String
    updateBook(id: ID!, title: String, author: String, price: Float, quantity
