# Unit IV - U4Express Q&A Guide

## 5 MARKS QUESTIONS

### 1. Differentiate between Request Object (req) and Response Object (res) in Express

**Answer:**

The **Request Object (req)** and **Response Object (res)** are fundamental to Express routing:

**Request Object (req):**
- Contains data **sent by the client** to the server
- Includes: HTTP method, URL, headers, body, query parameters, route parameters
- Examples: `req.body`, `req.params`, `req.query`, `req.headers`
- Used to **read/receive** information from the client

**Response Object (res):**
- Used to **send data back** to the client
- Includes methods: `res.send()`, `res.json()`, `res.status()`, `res.render()`
- Controls the HTTP response: status codes, headers, body
- Used to **write/send** information to the client

**Key Difference:** `req` = incoming data (read), `res` = outgoing data (write)

---

### 2. Explain RESTful API design principles with suitable examples

**Answer:**

**Core RESTful Principles:**

1. **Client-Server Architecture:** Separation of concerns - client handles UI, server handles data
   
2. **Stateless:** Each request contains all necessary information; server doesn't store client state
   - Example: Send auth token with every request, not rely on server sessions

3. **Uniform Interface:** Consistent resource identification using URIs
   - Example: `/api/users/123` identifies user with ID 123

4. **Resource-Based:** Everything is a resource with unique URI
   - Users: `/users`, Products: `/products`

5. **HTTP Methods for CRUD:**
   - GET `/users` - Retrieve all users
   - POST `/users` - Create new user
   - PUT `/users/5` - Update user 5
   - DELETE `/users/5` - Delete user 5

6. **JSON Response Format:** Standardized data format
   ```json
   { "id": 1, "name": "John", "email": "john@example.com" }
   ```

---

### 3. Explain the role of middleware in Express

**Answer:**

**Middleware** functions have access to `req`, `res`, and `next()` in the request-response cycle.

**Key Roles:**

1. **Execute code** before reaching route handler
2. **Modify req/res objects** (add properties, parse data)
3. **End request-response cycle** (send response)
4. **Call next middleware** using `next()`

**Types & Examples:**

- **Application-level:** `app.use(express.json())` - parses JSON
- **Route-level:** Applied to specific routes
- **Error-handling:** Has 4 parameters: `(err, req, res, next)`
- **Built-in:** `express.static()`, `express.json()`
- **Third-party:** `cors()`, `morgan()`

**Example:**
```javascript
// Logging middleware
app.use((req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next(); // Pass to next middleware
});
```

**Remember:** Middleware executes in the **order it's defined**.

---

### 4. Describe the role of Express in building REST APIs and route handler functions

**Answer:**

**Express** is a minimal Node.js framework that simplifies REST API development.

**Role in REST APIs:**

1. **Routing:** Maps HTTP methods + URLs to handler functions
   ```javascript
   app.get('/api/users', getAllUsers);
   app.post('/api/users', createUser);
   ```

2. **Request Handling:** Provides easy access to request data
   - Body: `req.body`
   - Parameters: `req.params`
   - Query: `req.query`

3. **Response Management:** Simple methods to send responses
   - `res.json()` - Send JSON
   - `res.status()` - Set status code
   - `res.send()` - Send response

**Route Handler Functions:**
- Functions that execute when a route is matched
- Signature: `(req, res, next) => {}`
- Handle business logic and send responses

**Example:**
```javascript
app.get('/users/:id', (req, res) => {
    const user = findUser(req.params.id);
    res.json(user);
});
```

---

### 5. Show how to use res.status() and res.json() together in Express

**Answer:**

**Method Chaining:** `res.status()` returns the response object, allowing chaining with `res.json()`.

**Syntax:**
```javascript
res.status(statusCode).json(jsonData);
```

**Common Examples:**

**Success (200):**
```javascript
res.status(200).json({ message: "Success", data: user });
```

**Created (201):**
```javascript
res.status(201).json({ message: "User created", user: newUser });
```

**Not Found (404):**
```javascript
res.status(404).json({ error: "User not found" });
```

**Bad Request (400):**
```javascript
res.status(400).json({ error: "Invalid input" });
```

**Server Error (500):**
```javascript
res.status(500).json({ error: "Server error" });
```

**Why use both?**
- `status()` sets HTTP status code (for client to understand result)
- `json()` sends data in JSON format (structured response)
- Together they provide **complete, meaningful responses**

---

### 6. Write a route handler for GET /orders/:id that retrieves an order by ID

**Answer:**

```javascript
const express = require('express');
const app = express();

// Sample orders database
const orders = [
    { id: 1, item: "Laptop", price: 1200 },
    { id: 2, item: "Mouse", price: 25 },
    { id: 3, item: "Keyboard", price: 75 }
];

// Route handler for GET /orders/:id
app.get('/orders/:id', (req, res) => {
    // Extract id from URL parameter
    const orderId = parseInt(req.params.id);
    
    // Find order in database
    const order = orders.find(o => o.id === orderId);
    
    // Check if order exists
    if (!order) {
        return res.status(404).json({ 
            error: "Order not found" 
        });
    }
    
    // Send order data
    res.status(200).json({ 
        success: true, 
        order: order 
    });
});

app.listen(3000);
```

**Key Points:**
- `req.params.id` extracts dynamic ID from URL
- `parseInt()` converts string to number
- Returns 404 if not found, 200 if found

---

### 7. Show how to handle invalid JSON input using error handling middleware

**Answer:**

```javascript
const express = require('express');
const app = express();

// Parse JSON (can throw errors for invalid JSON)
app.use(express.json());

// Regular route
app.post('/users', (req, res) => {
    const { name, email } = req.body;
    res.status(201).json({ message: "User created", name, email });
});

// Error handling middleware (must have 4 parameters)
app.use((err, req, res, next) => {
    // Check if it's a JSON parsing error
    if (err instanceof SyntaxError && err.status === 400 && 'body' in err) {
        return res.status(400).json({ 
            error: "Invalid JSON format",
            message: err.message 
        });
    }
    
    // Handle other errors
    console.error(err.stack);
    res.status(500).json({ 
        error: "Internal server error" 
    });
});

app.listen(3000);
```

**Key Points:**
- Error middleware has **4 parameters**: `(err, req, res, next)`
- Must be defined **after all routes**
- Catches JSON parsing errors from `express.json()`
- Returns user-friendly error messages

---

### 8. Explain how MongoDB integrates with Express for RESTful APIs

**Answer:**

**Integration Process:**

**1. Install Mongoose (ODM - Object Data Modeling library):**
```bash
npm install mongoose
```

**2. Connect to MongoDB:**
```javascript
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/mydb');
```

**3. Define Schema & Model:**
```javascript
const userSchema = new mongoose.Schema({
    name: String,
    email: String,
    age: Number
});

const User = mongoose.model('User', userSchema);
```

**4. Use in Express Routes:**
```javascript
// CREATE
app.post('/users', async (req, res) => {
    const user = new User(req.body);
    await user.save();
    res.status(201).json(user);
});

// READ
app.get('/users', async (req, res) => {
    const users = await User.find();
    res.json(users);
});

// UPDATE
app.put('/users/:id', async (req, res) => {
    const user = await User.findByIdAndUpdate(req.params.id, req.body, {new: true});
    res.json(user);
});

// DELETE
app.delete('/users/:id', async (req, res) => {
    await User.findByIdAndDelete(req.params.id);
    res.json({ message: "Deleted" });
});
```

**Benefits:**
- **Async/Await** for clean asynchronous code
- **Schema validation** ensures data integrity
- **Built-in methods** for CRUD operations
- **Easy querying** with MongoDB queries

---

### 9. Differentiate between req.params and req.query in Express

**Answer:**

| Feature | req.params | req.query |
|---------|-----------|-----------|
| **Purpose** | Extract route parameters | Extract query string parameters |
| **URL Part** | Part of the path | After `?` in URL |
| **Example URL** | `/users/123` | `/users?age=25&city=NYC` |
| **Route Definition** | `app.get('/users/:id')` | `app.get('/users')` |
| **Access** | `req.params.id` | `req.query.age`, `req.query.city` |
| **Usage** | Identify specific resource | Filter/sort/paginate resources |
| **Required** | Often required | Always optional |

**Example:**

```javascript
// req.params - Route parameters
app.get('/users/:id', (req, res) => {
    const userId = req.params.id; // "123"
    res.json({ userId });
});
// URL: /users/123

// req.query - Query parameters
app.get('/products', (req, res) => {
    const { category, minPrice } = req.query;
    // category = "electronics", minPrice = "100"
    res.json({ category, minPrice });
});
// URL: /products?category=electronics&minPrice=100

// Combined usage
app.get('/users/:id/posts', (req, res) => {
    const userId = req.params.id; // "5"
    const limit = req.query.limit; // "10"
    // Get posts for user 5, limit to 10
});
// URL: /users/5/posts?limit=10
```

**Memory Tip:** 
- **params** = **part** of path (mandatory identifier)
- **query** = **question** filters (optional filters)

---

### 10. Discuss why RESTful APIs should be stateless

**Answer:**

**Stateless** means each request contains **all necessary information**; the server doesn't remember previous requests.

**Why Stateless?**

**1. Scalability:**
- Any server can handle any request
- Easy to add more servers (horizontal scaling)
- No need to sync session data across servers

**2. Reliability:**
- Server crash doesn't lose client state
- Requests can be retried without side effects

**3. Simplicity:**
- Server code is simpler (no session management)
- Easy to cache responses
- Easier debugging (each request is independent)

**4. Load Balancing:**
- Requests can go to any server
- No "sticky sessions" needed

**Example:**

**Stateful (Bad):**
```javascript
// Server remembers user logged in
app.get('/profile', (req, res) => {
    if (session.userId) { // Depends on server memory
        res.json(getUserProfile(session.userId));
    }
});
```

**Stateless (Good):**
```javascript
// Client sends token with every request
app.get('/profile', (req, res) => {
    const token = req.headers.authorization; // All info in request
    const userId = verifyToken(token);
    res.json(getUserProfile(userId));
});
```

**Key Point:** Client maintains state (token), server just processes requests based on what's sent.

---

### 11. Define REST. Write two key principles and Map HTTP methods to CRUD operations

**Answer:**

**REST (Representational State Transfer):**
A software architectural style for building web services that use HTTP methods to perform operations on resources identified by URIs.

**Two Key Principles:**

1. **Resource-Based:** 
   - Everything is a resource (users, products, orders)
   - Each resource has a unique URI (e.g., `/users/123`)

2. **Stateless Communication:**
   - Each request contains all necessary information
   - Server doesn't store client context between requests

**HTTP Methods → CRUD Mapping:**

| CRUD | HTTP Method | Example | Description |
|------|-------------|---------|-------------|
| **C**reate | POST | `POST /users` | Create new resource |
| **R**ead | GET | `GET /users/5` | Retrieve resource(s) |
| **U**pdate | PUT/PATCH | `PUT /users/5` | Update entire/partial resource |
| **D**elete | DELETE | `DELETE /users/5` | Remove resource |

**Example:**

```javascript
const express = require('express');
const app = express();

// CREATE - POST
app.post('/products', (req, res) => {
    const product = { id: 1, name: "Laptop", price: 1200 };
    res.status(201).json(product);
});

// READ - GET
app.get('/products/:id', (req, res) => {
    const product = { id: req.params.id, name: "Laptop" };
    res.json(product);
});

// UPDATE - PUT
app.put('/products/:id', (req, res) => {
    const updated = { id: req.params.id, ...req.body };
    res.json(updated);
});

// DELETE - DELETE
app.delete('/products/:id', (req, res) => {
    res.json({ message: "Product deleted" });
});
```

---

### 12. State any two features of the Express framework. Explain briefly.

**Answer:**

**Feature 1: Middleware Support**

Express provides a powerful middleware system that allows you to execute functions in the request-response cycle.

- Middleware can modify `req` and `res` objects
- Can end the request or pass control to next middleware
- Used for logging, authentication, parsing, error handling

**Example:**
```javascript
// Middleware to log requests
app.use((req, res, next) => {
    console.log(`${req.method} ${req.url} - ${new Date()}`);
    next();
});

// Middleware to parse JSON
app.use(express.json());
```

**Feature 2: Routing**

Express provides a robust routing mechanism to handle different HTTP methods and URL patterns.

- Supports route parameters (`:id`)
- Modular route handlers
- Route chaining for same path
- Router-level middleware

**Example:**
```javascript
// Simple routes
app.get('/users', (req, res) => { res.json(users); });
app.post('/users', (req, res) => { /* create user */ });

// Route parameters
app.get('/users/:id', (req, res) => {
    const user = findUser(req.params.id);
    res.json(user);
});

// Modular routing
const router = express.Router();
router.get('/', getUsers);
router.post('/', createUser);
app.use('/api/users', router);
```

---

### 13. Write an Express route handler function for GET /hello that sends "Hello World!"

**Answer:**

```javascript
const express = require('express');
const app = express();

// Route handler for GET /hello
app.get('/hello', (req, res) => {
    res.send("Hello World!");
});

// Start server
app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

**Alternative with JSON response:**
```javascript
app.get('/hello', (req, res) => {
    res.json({ message: "Hello World!" });
});
```

**Alternative with status code:**
```javascript
app.get('/hello', (req, res) => {
    res.status(200).send("Hello World!");
});
```

**Testing:**
```
URL: http://localhost:3000/hello
Method: GET
Response: "Hello World!"
```

---

## 6 MARKS QUESTIONS

### 1. Write an Express route handler for GET /students/:id with error handling

**Answer:**

```javascript
const express = require('express');
const app = express();

// Sample student database
const students = [
    { id: 1, name: "Alice", grade: "A", course: "CS" },
    { id: 2, name: "Bob", grade: "B", course: "IT" },
    { id: 3, name: "Charlie", grade: "A", course: "CS" }
];

// Route handler for GET /students/:id
app.get('/students/:id', (req, res) => {
    // a) Retrieve student using req.params
    const studentId = parseInt(req.params.id);
    
    // Find student in database
    const student = students.find(s => s.id === studentId);
    
    // c) Handle case where student is not found
    if (!student) {
        return res.status(404).json({
            success: false,
            error: "Student not found",
            message: `No student exists with ID ${studentId}`
        });
    }
    
    // b) Send response in JSON format
    res.status(200).json({
        success: true,
        data: student
    });
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

**Explanation:**

**a) Retrieves student using req.params:**
- `req.params.id` extracts the dynamic ID from URL
- `parseInt()` converts string to number for comparison
- `find()` searches array for matching student

**b) Sends response in JSON format:**
- Uses `res.json()` to send JSON response
- Includes `success` flag and `data` object
- Status 200 for successful retrieval

**c) Handles not found case:**
- Checks if `student` is undefined
- Returns 404 status code (Not Found)
- Sends descriptive error message in JSON format
- Uses `return` to prevent further execution

**Testing:**
```
// Success case
GET /students/1
Response: { "success": true, "data": { "id": 1, "name": "Alice", ... } }

// Not found case
GET /students/999
Response: { "success": false, "error": "Student not found", ... }
```

---

### 2. Write an Express route handler for POST /orders with MongoDB integration

**Answer:**

```javascript
const express = require('express');
const mongoose = require('mongoose');
const app = express();

// Parse JSON
app.use(express.json());

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/orderdb');

// a) Define Order Schema
const orderSchema = new mongoose.Schema({
    orderId: { type: String, required: true, unique: true },
    items: [{ 
        name: String, 
        quantity: Number, 
        price: Number 
    }],
    totalAmount: { type: Number, required: true },
    createdAt: { type: Date, default: Date.now }
});

const Order = mongoose.model('Order', orderSchema);

// b) Route handler for POST /orders
app.post('/orders', async (req, res) => {
    try {
        // Accept JSON payload with orderId, items, and totalAmount
        const { orderId, items, totalAmount } = req.body;
        
        // Validate required fields
        if (!orderId || !items || !totalAmount) {
            return res.status(400).json({
                success: false,
                error: "Missing required fields: orderId, items, totalAmount"
            });
        }
        
        // Create new order instance
        const newOrder = new Order({
            orderId,
            items,
            totalAmount
        });
        
        // Save order to MongoDB
        const savedOrder = await newOrder.save();
        
        // c) Return success response in JSON
        res.status(201).json({
            success: true,
            message: "Order created successfully",
            data: savedOrder
        });
        
    } catch (error) {
        // Handle errors (duplicate orderId, validation errors, etc.)
        if (error.code === 11000) {
            return res.status(409).json({
                success: false,
                error: "Order ID already exists"
            });
        }
        
        res.status(500).json({
            success: false,
            error: "Failed to create order",
            message: error.message
        });
    }
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

**Sample JSON Payload:**
```json
{
    "orderId": "ORD-2024-001",
    "items": [
        { "name": "Laptop", "quantity": 1, "price": 1200 },
        { "name": "Mouse", "quantity": 2, "price": 25 }
    ],
    "totalAmount": 1250
}
```

**Explanation:**

**a) Accepts JSON payload:**
- Uses `express.json()` middleware to parse request body
- Destructures `orderId`, `items`, `totalAmount` from `req.body`
- Validates required fields before processing

**b) Saves to MongoDB:**
- Creates new `Order` instance with received data
- Uses `await newOrder.save()` for asynchronous database operation
- Mongoose automatically validates against schema

**c) Returns success response:**
- Status 201 (Created) for successful creation
- Includes success flag, message, and saved order data
- Handles errors like duplicate IDs (409) and validation errors (500)

---

## 10 MARKS QUESTIONS

### 2. Create a RESTful API for employee payroll with salary validation middleware

**Answer:**

```javascript
const express = require('express');
const mongoose = require('mongoose');
const app = express();

app.use(express.json());

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/payrolldb');

// Employee Schema
const employeeSchema = new mongoose.Schema({
    employeeId: { type: String, required: true, unique: true },
    name: { type: String, required: true },
    department: { type: String, required: true },
    salary: { type: Number, required: true, min: 0 },
    joinDate: { type: Date, default: Date.now }
});

const Employee = mongoose.model('Employee', employeeSchema);

// MIDDLEWARE: Salary Validation
const validateSalary = (req, res, next) => {
    const { salary } = req.body;
    
    // Check if salary exists
    if (salary === undefined || salary === null) {
        return res.status(400).json({
            success: false,
            error: "Salary is required"
        });
    }
    
    // Check if salary is a number
    if (typeof salary !== 'number' && isNaN(Number(salary))) {
        return res.status(400).json({
            success: false,
            error: "Salary must be a valid number"
        });
    }
    
    // Check if salary is positive
    if (Number(salary) <= 0) {
        return res.status(400).json({
            success: false,
            error: "Salary must be greater than 0"
        });
    }
    
    // Check reasonable maximum (optional)
    if (Number(salary) > 10000000) {
        return res.status(400).json({
            success: false,
            error: "Salary exceeds maximum limit"
        });
    }
    
    // Validation passed, proceed to next middleware/route handler
    next();
};

// MIDDLEWARE: Request Logging
const logRequest = (req, res, next) => {
    console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
    next();
};

// Apply logging to all routes
app.use(logRequest);

// ROUTE 1: LIST ALL EMPLOYEES - GET /employees
app.get('/employees', async (req, res) => {
    try {
        // Optional query parameters for filtering
        const { department, minSalary, maxSalary } = req.query;
        
        // Build query object
        let query = {};
        
        if (department) {
            query.department = department;
        }
        
        if (minSalary || maxSalary) {
            query.salary = {};
            if (minSalary) query.salary.$gte = Number(minSalary);
            if (maxSalary) query.salary.$lte = Number(maxSalary);
        }
        
        // Fetch employees from database
        const employees = await Employee.find(query).sort({ name: 1 });
        
        res.status(200).json({
            success: true,
            count: employees.length,
            data: employees
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to fetch employees",
            message: error.message
        });
    }
});

// ROUTE 2: GET EMPLOYEE BY ID - GET /employees/:id
app.get('/employees/:id', async (req, res) => {
    try {
        const employee = await Employee.findOne({ 
            employeeId: req.params.id 
        });
        
        if (!employee) {
            return res.status(404).json({
                success: false,
                error: "Employee not found"
            });
        }
        
        res.status(200).json({
            success: true,
            data: employee
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to fetch employee",
            message: error.message
        });
    }
});

// ROUTE 3: ADD NEW EMPLOYEE - POST /employees
// Apply salary validation middleware
app.post('/employees', validateSalary, async (req, res) => {
    try {
        const { employeeId, name, department, salary } = req.body;
        
        // Validate required fields
        if (!employeeId || !name || !department) {
            return res.status(400).json({
                success: false,
                error: "Missing required fields: employeeId, name, department"
            });
        }
        
        // Create new employee
        const newEmployee = new Employee({
            employeeId,
            name,
            department,
            salary: Number(salary)
        });
        
        // Save to database
        const savedEmployee = await newEmployee.save();
        
        res.status(201).json({
            success: true,
            message: "Employee added successfully",
            data: savedEmployee
        });
        
    } catch (error) {
        // Handle duplicate employeeId
        if (error.code === 11000) {
            return res.status(409).json({
                success: false,
                error: "Employee ID already exists"
            });
        }
        
        res.status(500).json({
            success: false,
            error: "Failed to add employee",
            message: error.message
        });
    }
});

// ROUTE 4: UPDATE EMPLOYEE - PUT /employees/:id
app.put('/employees/:id', validateSalary, async (req, res) => {
    try {
        const { name, department, salary } = req.body;
        
        const updatedEmployee = await Employee.findOneAndUpdate(
            { employeeId: req.params.id },
            { name, department, salary: Number(salary) },
            { new: true, runValidators: true }
        );
        
        if (!updatedEmployee) {
            return res.status(404).json({
                success: false,
                error: "Employee not found"
            });
        }
        
        res.status(200).json({
            success: true,
            message: "Employee updated successfully",
            data: updatedEmployee
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to update employee",
            message: error.message
        });
    }
});

// ROUTE 5: DELETE EMPLOYEE - DELETE /employees/:id
app.delete('/employees/:id', async (req, res) => {
    try {
        const deletedEmployee = await Employee.findOneAndDelete({ 
            employeeId: req.params.id 
        });
        
        if (!deletedEmployee) {
            return res.status(404).json({
                success: false,
                error: "Employee not found"
            });
        }
        
        res.status(200).json({
            success: true,
            message: "Employee deleted successfully",
            data: deletedEmployee
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to delete employee",
            message: error.message
        });
    }
});

// Error handling middleware
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({
        success: false,
        error: "Internal server error",
        message: err.message
    });
});

// Start server
const PORT = 3000;
app.listen(PORT, () => {
    console.log(`Employee Payroll API running on port ${PORT}`);
});
```

**Sample Requests:**

**1. List All Employees:**
```
GET /employees
Response: { "success": true, "count": 5, "data": [...] }
```

**2. Filter by Department:**
```
GET /employees?department=IT&minSalary=50000
```

**3. Add New Employee:**
```
POST /employees
Body: {
    "employeeId": "EMP001",
    "name": "John Doe",
    "department": "IT",
    "salary": 75000
}
```

**4. Invalid Salary (Middleware catches):**
```
POST /employees
Body: { "salary": -5000 }
Response: { "error": "Salary must be greater than 0" }
```

**Key Features:**
- ✅ Complete CRUD operations
- ✅ Salary validation middleware (> 0)
- ✅ Request logging middleware
- ✅ Query parameter filtering
- ✅ Error handling for duplicates
- ✅ MongoDB integration with Mongoose
- ✅ Proper HTTP status codes

---

### 3. Design a RESTful API for event management system with date validation

**Answer:**

```javascript
const express = require('express');
const mongoose = require('mongoose');
const app = express();

app.use(express.json());

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/eventdb');

// Event Schema
const eventSchema = new mongoose.Schema({
    eventId: { type: String, required: true, unique: true },
    title: { type: String, required: true },
    description: { type: String },
    date: { type: Date, required: true },
    location: { type: String, required: true },
    capacity: { type: Number, required: true },
    registeredCount: { type: Number, default: 0 },
    status: { 
        type: String, 
        enum: ['upcoming', 'ongoing', 'completed', 'cancelled'],
        default: 'upcoming'
    },
    createdAt: { type: Date, default: Date.now }
});

const Event = mongoose.model('Event', eventSchema);

// MIDDLEWARE: Date Validation
const validateEventDate = (req, res, next) => {
    const { date } = req.body;
    
    // Check if date exists
    if (!date) {
        return res.status(400).json({
            success: false,
            error: "Event date is required"
        });
    }
    
    // Parse and validate date
    const eventDate = new Date(date);
    
    // Check if valid date
    if (isNaN(eventDate.getTime())) {
        return res.status(400).json({
            success: false,
            error: "Invalid date format. Use ISO 8601 format (YYYY-MM-DD)"
        });
    }
    
    // Check if date is in the future
    const today = new Date();
    today.setHours(0, 0, 0, 0); // Reset time to start of day
    
    if (eventDate < today) {
        return res.status(400).json({
            success: false,
            error: "Event date must be in the future or today"
        });
    }
    
    // Check if date is not too far in future (e.g., max 2 years)
    const maxDate = new Date();
    maxDate.setFullYear(maxDate.getFullYear() + 2);
    
    if (eventDate > maxDate) {
        return res.status(400).json({
            success: false,
            error: "Event date cannot be more than 2 years in the future"
        });
    }
    
    // Store validated date in req object
    req.validatedDate = eventDate;
    next();
};

// MIDDLEWARE: Capacity Validation
const validateCapacity = (req, res, next) => {
    const { capacity } = req.body;
    
    if (capacity !== undefined) {
        if (!Number.isInteger(Number(capacity)) || Number(capacity) <= 0) {
            return res.status(400).json({
                success: false,
                error: "Capacity must be a positive integer"
            });
        }
    }
    
    next();
};

// MIDDLEWARE: Request Logger
const logRequest = (req, res, next) => {
    console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
    next();
};

app.use(logRequest);

// ROUTE 1: LIST ALL EVENTS - GET /events
app.get('/events', async (req, res) => {
    try {
        // Query parameters for filtering
        const { status, upcoming, location } = req.query;
        
        let query = {};
        
        // Filter by status
        if (status) {
            query.status = status;
        }
        
        // Show only upcoming events
        if (upcoming === 'true') {
            query.date = { $gte: new Date() };
            query.status = 'upcoming';
        }
        
        // Filter by location
        if (location) {
            query.location = new RegExp(location, 'i'); // Case-insensitive
        }
        
        // Fetch events
        const events = await Event.find(query).sort({ date: 1 });
        
        res.status(200).json({
            success: true,
            count: events.length,
            data: events
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to fetch events",
            message: error.message
        });
    }
});

// ROUTE 2: GET EVENT BY ID - GET /events/:id
app.get('/events/:id', async (req, res) => {
    try {
        const event = await Event.findOne({ eventId: req.params.id });
        
        if (!event) {
            return res.status(404).json({
                success: false,
                error: "Event not found"
            });
        }
        
        // Calculate available seats
        const availableSeats = event.capacity - event.registeredCount;
        
        res.status(200).json({
            success: true,
            data: {
                ...event.toObject(),
                availableSeats
            }
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to fetch event",
            message: error.message
        });
    }
});

// ROUTE 3: CREATE NEW EVENT - POST /events
app.post('/events', validateEventDate, validateCapacity, async (req, res) => {
    try {
        const { eventId, title, description, location, capacity } = req.body;
        
        // Validate required fields
        if (!eventId || !title || !location || !capacity) {
            return res.status(400).json({
                success: false,
                error: "Missing required fields: eventId, title, location, capacity"
            });
        }
        
        // Create new event with validated date
        const newEvent = new Event({
            eventId,
            title,
            description,
            date: req.validatedDate, // Use validated date from middleware
            location,
            capacity: Number(capacity)
        });
        
        // Save to database
        const savedEvent = await newEvent.save();
        
        res.status(201).json({
            success: true,
            message: "Event created successfully",
            data: savedEvent
        });
        
    } catch (error) {
        if (error.code === 11000) {
            return res.status(409).json({
                success: false,
                error: "Event ID already exists"
            });
        }
        
        res.status(500).json({
            success: false,
            error: "Failed to create event",
            message: error.message
        });
    }
});

// ROUTE 4: UPDATE EVENT - PUT /events/:id
app.put('/events/:id', validateEventDate, validateCapacity, async (req, res) => {
    try {
        const { title, description, location, capacity, status } = req.body;
        
        const updateData = {
            title,
            description,
            date: req.validatedDate,
            location,
            capacity: Number(capacity),
            status
        };
        
        const updatedEvent = await Event.findOneAndUpdate(
            { eventId: req.params.id },
            updateData,
            { new: true, runValidators: true }
        );
        
        if (!updatedEvent) {
            return res.status(404).json({
                success: false,
                error: "Event not found"
            });
        }
        
        res.status(200).json({
            success: true,
            message: "Event updated successfully",
            data: updatedEvent
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to update event",
            message: error.message
        });
    }
});

// ROUTE 5: DELETE EVENT - DELETE /events/:id
app.delete('/events/:id', async (req, res) => {
    try {
        const deletedEvent = await Event.findOneAndDelete({ 
            eventId: req.params.id 
        });
        
        if (!deletedEvent) {
            return res.status(404).json({
                success: false,
                error: "Event not found"
            });
        }
        
        res.status(200).json({
            success: true,
            message: "Event deleted successfully",
            data: deletedEvent
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to delete event",
            message: error.message
        });
    }
});

// ROUTE 6: REGISTER FOR EVENT - POST /events/:id/register
app.post('/events/:id/register', async (req, res) => {
    try {
        const event = await Event.findOne({ eventId: req.params.id });
        
        if (!event) {
            return res.status(404).json({
                success: false,
                error: "Event not found"
            });
        }
        
        // Check if event is full
        if (event.registeredCount >= event.capacity) {
            return res.status(400).json({
                success: false,
                error: "Event is full. No seats available."
            });
        }
        
        // Check if event date has passed
        if (event.date < new Date()) {
            return res.status(400).json({
                success: false,
                error: "Cannot register for past events"
            });
        }
        
        // Increment registered count
        event.registeredCount += 1;
        await event.save();
        
        res.status(200).json({
            success: true,
            message: "Successfully registered for event",
            data: {
                eventId: event.eventId,
                title: event.title,
                registeredCount: event.registeredCount,
                availableSeats: event.capacity - event.registeredCount
            }
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Registration failed",
            message: error.message
        });
    }
});

// Error handling middleware
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({
        success: false,
        error: "Internal server error"
    });
});

const PORT = 3000;
app.listen(PORT, () => {
    console.log(`Event Management API running on port ${PORT}`);
});
```

**Sample Requests:**

**1. List All Events:**
```
GET /events
GET /events?upcoming=true
GET /events?location=Mumbai
```

**2. Create Event:**
```
POST /events
Body: {
    "eventId": "EVT001",
    "title": "Tech Conference 2024",
    "description": "Annual technology conference",
    "date": "2024-12-15",
    "location": "Mumbai",
    "capacity": 500
}
```

**3. Invalid Date (Past):**
```
POST /events
Body: { "date": "2020-01-01" }
Response: { "error": "Event date must be in the future or today" }
```

**4. Register for Event:**
```
POST /events/EVT001/register
```

**Key Features:**
- ✅ Date validation middleware (future dates only)
- ✅ Capacity validation
- ✅ Event registration with seat availability check
- ✅ Status management (upcoming/ongoing/completed)
- ✅ Query filtering (status, location, upcoming)
- ✅ Complete CRUD operations
- ✅ Error handling

---

### 6. Design a RESTful API for Customer Management System

**Answer:**

```javascript
const express = require('express');
const mongoose = require('mongoose');
const app = express();

app.use(express.json());

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/customerdb');

// a) Mongoose Schema for Customer
const customerSchema = new mongoose.Schema({
    customerId: {
        type: String,
        required: [true, 'Customer ID is required'],
        unique: true,
        trim: true
    },
    name: {
        type: String,
        required: [true, 'Customer name is required'],
        trim: true,
        minlength: [2, 'Name must be at least 2 characters long']
    },
    email: {
        type: String,
        required: [true, 'Email is required'],
        unique: true,
        trim: true,
        lowercase: true,
        match: [/^\S+@\S+\.\S+$/, 'Please provide a valid email address']
    },
    phone: {
        type: String,
        trim: true
    },
    address: {
        type: String
    },
    createdAt: {
        type: Date,
        default: Date.now
    }
});

const Customer = mongoose.model('Customer', customerSchema);

// c) Express Middleware - Validate Email Presence
const validateEmail = (req, res, next) => {
    const { email } = req.body;
    
    // Check if email is present
    if (!email) {
        return res.status(400).json({
            success: false,
            error: "Email is required",
            message: "Please provide an email address in the request body"
        });
    }
    
    // Check if email is not empty string
    if (email.trim() === '') {
        return res.status(400).json({
            success: false,
            error: "Email cannot be empty"
        });
    }
    
    // Basic email format validation
    const emailRegex = /^\S+@\S+\.\S+$/;
    if (!emailRegex.test(email)) {
        return res.status(400).json({
            success: false,
            error: "Invalid email format",
            message: "Please provide a valid email address"
        });
    }
    
    // Email validation passed
    next();
};

// Middleware: Validate Customer ID
const validateCustomerId = (req, res, next) => {
    const { customerId } = req.body;
    
    if (!customerId) {
        return res.status(400).json({
            success: false,
            error: "Customer ID is required"
        });
    }
    
    next();
};

// ROUTE 1: GET /customers - List all customers
app.get('/customers', async (req, res) => {
    try {
        // Optional query parameters
        const { search, sortBy } = req.query;
        
        let query = {};
        
        // Search by name or email
        if (search) {
            query.$or = [
                { name: new RegExp(search, 'i') },
                { email: new RegExp(search, 'i') }
            ];
        }
        
        // Fetch customers
        let customersQuery = Customer.find(query);
        
        // Sorting
        if (sortBy === 'name') {
            customersQuery = customersQuery.sort({ name: 1 });
        } else if (sortBy === 'recent') {
            customersQuery = customersQuery.sort({ createdAt: -1 });
        }
        
        const customers = await customersQuery;
        
        res.status(200).json({
            success: true,
            count: customers.length,
            data: customers
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to fetch customers",
            message: error.message
        });
    }
});

// ROUTE 2: GET /customers/:id - Get customer by ID
app.get('/customers/:id', async (req, res) => {
    try {
        const customer = await Customer.findOne({ 
            customerId: req.params.id 
        });
        
        if (!customer) {
            return res.status(404).json({
                success: false,
                error: "Customer not found",
                message: `No customer found with ID: ${req.params.id}`
            });
        }
        
        res.status(200).json({
            success: true,
            data: customer
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to fetch customer",
            message: error.message
        });
    }
});

// ROUTE 3: POST /customers - Add new customer
// Apply validation middleware
app.post('/customers', validateEmail, validateCustomerId, async (req, res) => {
    try {
        const { customerId, name, email, phone, address } = req.body;
        
        // Check for duplicate email
        const existingCustomer = await Customer.findOne({ email });
        if (existingCustomer) {
            return res.status(409).json({
                success: false,
                error: "Duplicate email",
                message: "A customer with this email already exists"
            });
        }
        
        // Create new customer
        const newCustomer = new Customer({
            customerId,
            name,
            email,
            phone,
            address
        });
        
        // Save to database
        const savedCustomer = await newCustomer.save();
        
        res.status(201).json({
            success: true,
            message: "Customer created successfully",
            data: savedCustomer
        });
        
    } catch (error) {
        // Handle duplicate customerId
        if (error.code === 11000) {
            if (error.keyPattern.customerId) {
                return res.status(409).json({
                    success: false,
                    error: "Customer ID already exists"
                });
            }
            if (error.keyPattern.email) {
                return res.status(409).json({
                    success: false,
                    error: "Email already exists"
                });
            }
        }
        
        // Handle validation errors
        if (error.name === 'ValidationError') {
            return res.status(400).json({
                success: false,
                error: "Validation failed",
                message: error.message
            });
        }
        
        res.status(500).json({
            success: false,
            error: "Failed to create customer",
            message: error.message
        });
    }
});

// ROUTE 4: PUT /customers/:id - Update customer
app.put('/customers/:id', validateEmail, async (req, res) => {
    try {
        const { name, email, phone, address } = req.body;
        
        // Check if updating email and it already exists
        if (email) {
            const existingCustomer = await Customer.findOne({ 
                email, 
                customerId: { $ne: req.params.id } // Exclude current customer
            });
            
            if (existingCustomer) {
                return res.status(409).json({
                    success: false,
                    error: "Email already exists for another customer"
                });
            }
        }
        
        const updatedCustomer = await Customer.findOneAndUpdate(
            { customerId: req.params.id },
            { name, email, phone, address },
            { new: true, runValidators: true }
        );
        
        if (!updatedCustomer) {
            return res.status(404).json({
                success: false,
                error: "Customer not found"
            });
        }
        
        res.status(200).json({
            success: true,
            message: "Customer updated successfully",
            data: updatedCustomer
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to update customer",
            message: error.message
        });
    }
});

// ROUTE 5: DELETE /customers/:id - Delete customer
app.delete('/customers/:id', async (req, res) => {
    try {
        const deletedCustomer = await Customer.findOneAndDelete({ 
            customerId: req.params.id 
        });
        
        if (!deletedCustomer) {
            return res.status(404).json({
                success: false,
                error: "Customer not found"
            });
        }
        
        res.status(200).json({
            success: true,
            message: "Customer deleted successfully",
            data: deletedCustomer
        });
        
    } catch (error) {
        res.status(500).json({
            success: false,
            error: "Failed to delete customer",
            message: error.message
        });
    }
});

// Error handling middleware
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({
        success: false,
        error: "Internal server error"
    });
});

const PORT = 3000;
app.listen(PORT, () => {
    console.log(`Customer Management API running on port ${PORT}`);
});
```

**b) JSON Payload for Adding New Customer:**

```json
{
    "customerId": "CUST001",
    "name": "John Smith",
    "email": "john.smith@example.com",
    "phone": "+1-555-0123",
    "address": "123 Main Street, New York, NY 10001"
}
```

**Alternative Minimal Payload:**
```json
{
    "customerId": "CUST002",
    "name": "Jane Doe",
    "email": "jane.doe@example.com"
}
```

**Sample API Requests:**

**1. List All Customers:**
```
GET /customers
Response: { "success": true, "count": 10, "data": [...] }
```

**2. Search Customers:**
```
GET /customers?search=john&sortBy=name
```

**3. Add Customer:**
```
POST /customers
Body: { "customerId": "CUST001", "name": "John", "email": "john@example.com" }
```

**4. Missing Email (Middleware Catches):**
```
POST /customers
Body: { "customerId": "CUST001", "name": "John" }
Response: { "error": "Email is required" }
```

**5. Duplicate Email:**
```
POST /customers
Body: { "email": "existing@example.com" }
Response: { "error": "A customer with this email already exists" }
```

**Key Components:**

**a) Mongoose Schema:** ✅
- customerId (String, required, unique)
- name (String, required, min 2 chars)
- email (String, required, unique, validated)

**b) JSON Payload:** ✅
- Includes all required fields
- Optional fields (phone, address)

**c) Email Validation Middleware:** ✅
- Checks email presence
- Validates email format
- Returns 400 for missing/invalid email

**Additional Features:**
- ✅ Duplicate email handling (409 Conflict)
- ✅ Search and sort functionality
- ✅ Complete CRUD operations
- ✅ Comprehensive error handling

---

## Additional Important Questions

### Define the purpose of a List API in RESTful architecture (5 marks)

**Answer:**

**List API** (also called **Collection Endpoint**) retrieves multiple resources from the server.

**Purpose:**

1. **Retrieve All Resources:** Fetch complete collection of resources
   - Example: `GET /products` returns all products

2. **Enable Filtering:** Allow clients to filter results using query parameters
   - Example: `GET /products?category=electronics&price<100`

3. **Support Pagination:** Handle large datasets efficiently
   - Example: `GET /users?page=2&limit=20`

4. **Provide Sorting:** Allow ordering of results
   - Example: `GET /articles?sortBy=date&order=desc`

5. **Search Functionality:** Enable search across resources
   - Example: `GET /customers?search=john`

**Example Implementation:**

```javascript
// List API for products
app.get('/products', async (req, res) => {
    const { category, minPrice, maxPrice, page = 1, limit = 10 } = req.query;
    
    let query = {};
    
    // Filtering
    if (category) query.category = category;
    if (minPrice || maxPrice) {
        query.price = {};
        if (minPrice) query.price.$gte = Number(minPrice);
        if (maxPrice) query.price.$lte = Number(maxPrice);
    }
    
    // Pagination
    const skip = (page - 1) * limit;
    
    const products = await Product.find(query)
        .limit(Number(limit))
        .skip(skip)
        .sort({ createdAt: -1 });
    
    const total = await Product.countDocuments(query);
    
    res.json({
        success: true,
        data: products,
        pagination: {
            page: Number(page),
            limit: Number(limit),
            total: total,
            pages: Math.ceil(total / limit)
        }
    });
});
```

**Benefits:**
- Efficient data retrieval
- Flexible querying
- Better user experience
- Reduces multiple API calls

---

### Compare REST and GraphQL for listing employees (10 marks)

**Answer:**

**REST Approach:**

```javascript
// REST Route: GET /employees
app.get('/employees', async (req, res) => {
    const employees = await Employee.find();
    res.json({
        success: true,
        data: employees
    });
});

// Client receives ALL fields
// Response:
{
    "data": [
        {
            "id": 1,
            "name": "John Doe",
            "email": "john@example.com",
            "department": "IT",
            "salary": 75000,
            "joinDate": "2020-01-15",
            "address": "123 Main St",
            "phone": "555-0123"
        },
        // ... more employees
    ]
}
```

**GraphQL Approach:**

```javascript
// GraphQL Schema
const typeDefs = `
    type Employee {
        id: ID!
        name: String!
        email: String!
        department: String!
        salary: Float!
        joinDate: String
        address: String
        phone: String
    }
    
    type Query {
        employees: [Employee]
        employee(id: ID!): Employee
    }
`;

// GraphQL Resolver
const resolvers = {
    Query: {
        employees: async () => {
            return await Employee.find();
        },
        employee: async (_, { id }) => {
            return await Employee.findById(id);
        }
    }
};

// GraphQL Query (Client decides what fields to get)
query {
    employees {
        id
        name
        department
    }
}

// Client receives ONLY requested fields
// Response:
{
    "data": {
        "employees": [
            {
                "id": "1",
                "name": "John Doe",
                "department": "IT"
            },
            // ... more employees with only these 3 fields
        ]
    }
}
```

**Comparison Table:**

| Aspect | REST | GraphQL |
|--------|------|---------|
| **Data Fetching** | Fixed structure, all fields | Flexible, client specifies fields |
| **Multiple Resources** | Multiple endpoints/requests | Single request with nested queries |
| **Over-fetching** | Common (gets unnecessary data) | Eliminated (get exactly what you need) |
| **Under-fetching** | Common (need multiple requests) | Eliminated (get all data in one request) |
| **API Versioning** | Needs versioning (v1, v2) | No versioning needed (schema evolution) |
| **Caching** | Easy (HTTP caching) | Complex (needs custom solutions) |
| **Learning Curve** | Simple, well-known | Steeper learning curve |
| **Tooling** | Mature ecosystem | Growing ecosystem |

**Advantages of REST:**
1. ✅ **Simplicity:** Easy to understand and implement
2. ✅ **HTTP Caching:** Built-in browser and CDN caching
3. ✅ **Stateless:** Clear separation of concerns
4. ✅ **Standardized:** Well-established conventions
5. ✅ **Easy Testing:** Can use simple tools like curl, Postman

**Disadvantages of REST:**
1. ❌ **Over-fetching:** Gets more data than needed
2. ❌ **Under-fetching:** May need multiple requests
3. ❌ **Fixed Structure:** Can't customize response
4. ❌ **Versioning Issues:** API versions can multiply
5. ❌ **Multiple Endpoints:** Many routes to maintain

**Advantages of GraphQL:**
1. ✅ **Precise Data Fetching:** Get exactly what you need
2. ✅ **Single Request:** Fetch related data in one query
3. ✅ **Strong Typing:** Schema provides clear contract
4. ✅ **Self-Documenting:** Schema serves as documentation
5. ✅ **Real-time:** Built-in subscription support

**Disadvantages of GraphQL:**
1. ❌ **Complexity:** More complex to set up
2. ❌ **Caching Difficulty:** Harder to implement caching
3. ❌ **Query Complexity:** Can lead to expensive queries
4. ❌ **Learning Curve:** Requires understanding new concepts
5. ❌ **Overhead:** More setup for simple APIs

**When to Use:**

**Use REST when:**
- Simple CRUD operations
- Public APIs with predictable access patterns
- Need HTTP caching
- Small to medium applications
- Team familiar with REST

**Use GraphQL when:**
- Complex data requirements
- Mobile apps (reduce data transfer)
- Need flexible querying
- Multiple client types with different needs
- Rapidly evolving requirements

**Conclusion:**
REST is simpler and better for straightforward APIs. GraphQL excels when you need flexibility and efficiency in data fetching. Choose based on your specific requirements, team expertise, and application complexity.

---
