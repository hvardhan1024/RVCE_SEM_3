# Unit I - Full Stack Development Q&A Guide (Simplified)

## Part 1: Introduction to Full Stack Development

### 1. Define Full Stack Development and list its main components (5 marks)

**Answer:**

**Full Stack Development** = Building **both** front-end (what users see) and back-end (server/database) of a web application.

**Simple Analogy:**

- **Front-end** = Restaurant dining area (what customers see)
- **Back-end** = Kitchen (where food is prepared)
- **Full Stack Developer** = Someone who can do both!

**Main Components:**

**1. Front-End (Client Side)**

- **What users see and interact with**
- Technologies: HTML, CSS, JavaScript, React
- Runs in the browser

**2. Back-End (Server Side)**

- **Handles business logic and data**
- Technologies: Node.js, Express.js
- Runs on the server

**3. Database**

- **Stores all data**
- Technologies: MongoDB, MySQL, PostgreSQL
- Where information is saved

**4. APIs (Application Programming Interface)**

- **Communication bridge**
- Connects front-end to back-end
- RESTful APIs

**Visual Representation:**

```
┌─────────────────────────────────────┐
│     FRONT-END (Client)              │
│  - HTML/CSS/JavaScript              │
│  - React (User Interface)           │
└──────────────┬──────────────────────┘
               │
               │ HTTP Requests/Responses
               │
┌──────────────▼──────────────────────┐
│     BACK-END (Server)               │
│  - Node.js + Express.js             │
│  - Business Logic                   │
│  - API Routes                       │
└──────────────┬──────────────────────┘
               │
               │ Database Queries
               │
┌──────────────▼──────────────────────┐
│     DATABASE                        │
│  - MongoDB                          │
│  - Data Storage                     │
└─────────────────────────────────────┘
```

**Example:**

**E-commerce Website:**

- **Front-end**: Product display, shopping cart button
- **Back-end**: Process order, calculate total price
- **Database**: Store product details, customer info
- **API**: "Add to cart" sends request to server

**Remember:** Full Stack = Front-end + Back-end + Database

---

### 2. Explain stages in Full Stack Application Development cycle (6 marks)

**Answer:**

**Development Cycle** = Steps to build a complete application from start to finish.

**Stage 1: Planning & Requirements**

- Decide what the application will do
- Identify features needed
- Choose technologies (MERN stack)

**Example:**

```
Building a Todo App:
- Users can add tasks
- Users can mark tasks complete
- Users can delete tasks
```

**Stage 2: Design**

- Design user interface (UI/UX)
- Create database schema
- Plan API endpoints

**Example:**

```
UI Design: Simple list with checkboxes
Database: { id, task, completed, createdDate }
API: GET /tasks, POST /tasks, DELETE /tasks/:id
```

**Stage 3: Front-End Development**

- Build user interface using React
- Create components
- Handle user interactions

**Example:**

```javascript
// TodoList component
function TodoList() {
  return (
    <div>
      <input type="text" placeholder="Add task" />
      <button>Add</button>
      <ul>
        <li>Task 1 ✓</li>
        <li>Task 2</li>
      </ul>
    </div>
  )
}
```

**Stage 4: Back-End Development**

- Create server using Node.js + Express
- Build API routes
- Implement business logic

**Example:**

```javascript
// Express server
app.post("/tasks", (req, res) => {
  const task = req.body
  // Save to database
  res.json({ message: "Task added!" })
})
```

**Stage 5: Database Integration**

- Set up MongoDB
- Create schemas using Mongoose
- Connect back-end to database

**Example:**

```javascript
// MongoDB Schema
const taskSchema = new mongoose.Schema({
  task: String,
  completed: Boolean,
  createdDate: Date,
})
```

**Stage 6: Testing**

- Test each feature
- Fix bugs
- Ensure everything works together

**Example:**

```
✓ Can add task?
✓ Can delete task?
✓ Does data save to database?
```

**Stage 7: Deployment**

- Deploy front-end (Vercel, Netlify)
- Deploy back-end (Heroku, AWS)
- Deploy database (MongoDB Atlas)

**Complete Flow:**

```
Planning → Design → Front-end → Back-end → Database → Testing → Deployment
```

**Remember:** Each stage builds on the previous one!

---

### 3. Illustrate MVC architecture in MERN stack (10 marks)

**Answer:**

**MVC** = **Model-View-Controller** - A way to organize code into three parts.

**Think of a Restaurant:**

- **Model** = Recipe/Ingredients (Data)
- **View** = Plated food (What customer sees)
- **Controller** = Chef (Prepares and serves)

**MVC Architecture Diagram:**

```
┌─────────────────────────────────────────────────────────┐
│                      USER                               │
│             (Clicks, Types, Interacts)                  │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│                  VIEW (React)                           │
│  - User Interface                                       │
│  - Components (TodoList, TodoItem)                      │
│  - Displays data to user                                │
└────────────────────┬────────────────────────────────────┘
                     │
                     │ Sends request
                     ▼
┌─────────────────────────────────────────────────────────┐
│              CONTROLLER (Express Routes)                │
│  - Receives requests from View                          │
│  - Processes requests                                   │
│  - Calls Model to get/save data                         │
│  - Sends response back to View                          │
└────────────────────┬────────────────────────────────────┘
                     │
                     │ Interacts with
                     ▼
┌─────────────────────────────────────────────────────────┐
│              MODEL (Mongoose Schema)                    │
│  - Defines data structure                               │
│  - Interacts with MongoDB                               │
│  - Handles database operations                          │
└─────────────────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│                 DATABASE (MongoDB)                      │
│  - Stores actual data                                   │
└─────────────────────────────────────────────────────────┘
```

**Complete Example - Todo Application:**

**1. MODEL (models/Task.js)**

```javascript
const mongoose = require("mongoose")

// Define data structure
const taskSchema = new mongoose.Schema({
  title: {
    type: String,
    required: true,
  },
  completed: {
    type: Boolean,
    default: false,
  },
  createdAt: {
    type: Date,
    default: Date.now,
  },
})

// Create Model
const Task = mongoose.model("Task", taskSchema)

module.exports = Task
```

**2. CONTROLLER (routes/taskRoutes.js)**

```javascript
const express = require("express")
const router = express.Router()
const Task = require("../models/Task")

// Get all tasks
router.get("/tasks", async (req, res) => {
  try {
    // Ask Model to get data
    const tasks = await Task.find()

    // Send to View
    res.json(tasks)
  } catch (error) {
    res.status(500).json({ error: error.message })
  }
})

// Create new task
router.post("/tasks", async (req, res) => {
  try {
    // Get data from View
    const { title } = req.body

    // Ask Model to save data
    const newTask = await Task.create({ title })

    // Send response to View
    res.status(201).json(newTask)
  } catch (error) {
    res.status(500).json({ error: error.message })
  }
})

// Update task
router.put("/tasks/:id", async (req, res) => {
  try {
    const { id } = req.params
    const { completed } = req.body

    // Ask Model to update
    const updatedTask = await Task.findByIdAndUpdate(
      id,
      { completed },
      { new: true },
    )

    res.json(updatedTask)
  } catch (error) {
    res.status(500).json({ error: error.message })
  }
})

// Delete task
router.delete("/tasks/:id", async (req, res) => {
  try {
    await Task.findByIdAndDelete(req.params.id)
    res.json({ message: "Task deleted" })
  } catch (error) {
    res.status(500).json({ error: error.message })
  }
})

module.exports = router
```

**3. VIEW (React Component - TaskList.jsx)**

```javascript
import { useState, useEffect } from "react"

function TaskList() {
  const [tasks, setTasks] = useState([])
  const [newTask, setNewTask] = useState("")

  // Fetch tasks from Controller
  useEffect(() => {
    fetch("http://localhost:5000/tasks")
      .then((res) => res.json())
      .then((data) => setTasks(data))
  }, [])

  // Add task - Send to Controller
  const handleAddTask = () => {
    fetch("http://localhost:5000/tasks", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ title: newTask }),
    })
      .then((res) => res.json())
      .then((task) => {
        setTasks([...tasks, task])
        setNewTask("")
      })
  }

  // Toggle complete - Send to Controller
  const handleToggle = (id, completed) => {
    fetch(`http://localhost:5000/tasks/${id}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ completed: !completed }),
    })
      .then((res) => res.json())
      .then((updatedTask) => {
        setTasks(tasks.map((t) => (t._id === id ? updatedTask : t)))
      })
  }

  // Delete task - Send to Controller
  const handleDelete = (id) => {
    fetch(`http://localhost:5000/tasks/${id}`, {
      method: "DELETE",
    }).then(() => {
      setTasks(tasks.filter((t) => t._id !== id))
    })
  }

  return (
    <div>
      <h1>My Tasks</h1>

      {/* Add Task */}
      <input
        value={newTask}
        onChange={(e) => setNewTask(e.target.value)}
        placeholder="Enter task"
      />
      <button onClick={handleAddTask}>Add</button>

      {/* Display Tasks */}
      <ul>
        {tasks.map((task) => (
          <li key={task._id}>
            <input
              type="checkbox"
              checked={task.completed}
              onChange={() => handleToggle(task._id, task.completed)}
            />
            <span
              style={{
                textDecoration: task.completed ? "line-through" : "none",
              }}
            >
              {task.title}
            </span>
            <button onClick={() => handleDelete(task._id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  )
}

export default TaskList
```

**How MVC Works Together:**

```
User clicks "Add Task"
  ↓
VIEW (React) sends POST request to /tasks
  ↓
CONTROLLER receives request
  ↓
CONTROLLER calls MODEL to save data
  ↓
MODEL saves to MongoDB
  ↓
CONTROLLER sends response back
  ↓
VIEW updates and shows new task
```

**Benefits of MVC:**

- ✅ **Organized code** - Each part has one job
- ✅ **Easy to maintain** - Change one part without affecting others
- ✅ **Reusable** - Same Model for different Views
- ✅ **Team work** - Different people work on different parts

**Remember:**

- **Model** = Data (MongoDB + Mongoose)
- **View** = What users see (React)
- **Controller** = Logic (Express routes)

---

### 4. Benefits of well-organized folder structure in MVC (4 marks)

**Answer:**

**Folder Structure Example:**

```
project/
├── client/                 (Front-end - VIEW)
│   ├── src/
│   │   ├── components/
│   │   │   ├── TaskList.jsx
│   │   │   └── TaskItem.jsx
│   │   ├── App.js
│   │   └── index.js
│   └── package.json
│
├── server/                 (Back-end)
│   ├── models/            (MODEL)
│   │   └── Task.js
│   ├── routes/            (CONTROLLER)
│   │   └── taskRoutes.js
│   ├── controllers/
│   │   └── taskController.js
│   ├── config/
│   │   └── db.js
│   ├── server.js
│   └── package.json
│
└── README.md
```

**Benefits:**

**1. Easy to Find Files**

```
Need to change how tasks are displayed?
→ Go to client/src/components/TaskList.jsx

Need to add a new database field?
→ Go to server/models/Task.js

Need to add a new API route?
→ Go to server/routes/taskRoutes.js
```

**2. Team Collaboration**

```
Developer A: Works on client/components (Front-end)
Developer B: Works on server/models (Database)
Developer C: Works on server/routes (APIs)

No conflicts! Everyone knows where to work.
```

**3. Easier Debugging**

```
Bug in displaying tasks?
→ Check client/components/

Bug in saving data?
→ Check server/models/

Bug in API?
→ Check server/routes/
```

**4. Scalability**

```
Adding new feature: User Authentication

client/
  ├── components/
  │   ├── Login.jsx     ← Add here
  │   └── Register.jsx  ← Add here

server/
  ├── models/
  │   └── User.js       ← Add here
  ├── routes/
  │   └── authRoutes.js ← Add here
```

**Remember:** Good organization = Easy maintenance!

---

### 5. Single Page Applications (SPA) vs Traditional Web Apps (6 marks)

**Answer:**

**Single Page Application (SPA)** = Web app that loads **once** and updates content dynamically without reloading the page.

**Traditional Web App** = Loads a **new page** every time you click a link.

**Comparison:**

| Feature             | Traditional Web App         | Single Page App (SPA)               |
| ------------------- | --------------------------- | ----------------------------------- |
| **Page Reload**     | Full reload every click     | No reload, updates content          |
| **Speed**           | Slower (reloads everything) | Faster (updates only changed parts) |
| **User Experience** | Page flickers               | Smooth, app-like                    |
| **Technology**      | HTML pages from server      | JavaScript (React)                  |
| **Example**         | Wikipedia                   | Gmail, Facebook                     |

**How Traditional Web App Works:**

```
User clicks "About Us"
  ↓
Browser requests about.html from server
  ↓
Server sends entire about.html page
  ↓
Browser reloads completely (page flickers)
  ↓
New page displayed
```

**Visual:**

```
Home Page (home.html) → Click → About Page (about.html) → Click → Contact (contact.html)
   ↓ Full reload           ↓ Full reload              ↓ Full reload
```

**How Single Page App Works:**

```
User clicks "About Us"
  ↓
JavaScript changes content on same page
  ↓
Only "About" section updates
  ↓
No page reload! Smooth transition
```

**Visual:**

```
Single Page (index.html)
   ↓ Load once
Content changes dynamically:
   Home → About → Contact (Same page, different content)
```

**Code Example:**

**Traditional (Multiple HTML files):**

```html
<!-- home.html -->
<a href="about.html">About</a>
<!-- New page load -->

<!-- about.html -->
<h1>About Us</h1>
<a href="contact.html">Contact</a>
<!-- Another new page -->

<!-- contact.html -->
<h1>Contact</h1>
```

**SPA (React - One page):**

```javascript
// App.js - Single page, content changes
import { BrowserRouter, Routes, Route, Link } from "react-router-dom"

function App() {
  return (
    <BrowserRouter>
      <nav>
        {/* Links don't reload page */}
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/contact">Contact</Link>
      </nav>

      {/* Content changes here, no reload */}
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </BrowserRouter>
  )
}

function Home() {
  return <h1>Home Page</h1>
}

function About() {
  return <h1>About Us</h1>
}

function Contact() {
  return <h1>Contact Us</h1>
}
```

**Advantages of SPA:**

- ✅ Faster (no full page reload)
- ✅ Better UX (smooth like mobile app)
- ✅ Less server load
- ✅ Works offline (with service workers)

**Disadvantages of SPA:**

- ❌ Initial load slower (loads all JavaScript)
- ❌ SEO challenging (Google can't easily read)
- ❌ More JavaScript needed

**Real Examples:**

**Traditional:**

- Wikipedia
- News websites
- Blogs

**SPA:**

- Gmail (emails load without reload)
- Facebook (scroll infinitely, no reload)
- Netflix (browse movies smoothly)
- Google Maps (pan/zoom without reload)

**Remember:** SPA = One page that changes content, Traditional = Many pages that reload

---

### 6. List and explain four MERN components (4 marks)

**Answer:**

**MERN** = **M**ongoDB + **E**xpress + **R**eact + **N**ode.js

**1. MongoDB (Database)**

**What:** NoSQL database that stores data in JSON-like documents

**Purpose:** Store all application data

**Example:**

```javascript
// Storing user data
{
  _id: 1,
  name: "John Doe",
  email: "john@example.com",
  posts: [1, 2, 3]
}
```

**Why:** Flexible, scalable, works well with JavaScript

---

**2. Express.js (Back-End Framework)**

**What:** Framework for building web servers with Node.js

**Purpose:** Handle HTTP requests, create APIs, middleware

**Example:**

```javascript
const express = require("express")
const app = express()

// API route
app.get("/users", (req, res) => {
  res.json({ name: "John", age: 25 })
})

app.listen(5000)
```

**Why:** Makes Node.js development easier, handles routing

---

**3. React (Front-End Library)**

**What:** JavaScript library for building user interfaces

**Purpose:** Create interactive, dynamic UIs

**Example:**

```javascript
function Welcome() {
  return (
    <div>
      <h1>Hello, User!</h1>
      <button>Click Me</button>
    </div>
  )
}
```

**Why:** Component-based, fast, reusable

---

**4. Node.js (Runtime Environment)**

**What:** JavaScript runtime that runs on the server

**Purpose:** Execute JavaScript outside browser, build back-end

**Example:**

```javascript
// Run JavaScript on server
const http = require("http")

const server = http.createServer((req, res) => {
  res.end("Hello from Node.js!")
})

server.listen(3000)
```

**Why:** Use JavaScript for both front-end and back-end

---

**How They Work Together:**

```
React (Front-end)
   ↓ Sends request
Express (API Routes)
   ↓ Processes request
Node.js (Runs Express)
   ↓ Database query
MongoDB (Stores data)
   ↓ Returns data
Express → React (Shows to user)
```

**Remember:** MERN = MongoDB + Express + React + Node.js

---

### 7. MERN Architecture Diagram (5 marks)

**Answer:**

**Complete MERN Architecture:**

```
┌─────────────────────────────────────────────────────────┐
│                   CLIENT (Browser)                      │
│                                                         │
│  ┌───────────────────────────────────────────────┐    │
│  │          REACT (Front-End)                     │    │
│  │  - Components (UI)                             │    │
│  │  - State Management                            │    │
│  │  - User Interactions                           │    │
│  └─────────────────┬─────────────────────────────┘    │
│                    │                                    │
└────────────────────┼────────────────────────────────────┘
                     │
                     │ HTTP Requests
                     │ (GET, POST, PUT, DELETE)
                     │
┌────────────────────▼────────────────────────────────────┐
│              SERVER (Node.js + Express)                 │
│                                                         │
│  ┌──────────────────────────────────────────────┐     │
│  │         EXPRESS.JS (Back-End)                 │     │
│  │  - Route Handlers                             │     │
│  │  - Middleware                                 │     │
│  │  - Business Logic                             │     │
│  │                                               │     │
│  │  Routes:                                      │     │
│  │  GET    /api/tasks    → Get all tasks        │     │
│  │  POST   /api/tasks    → Create task          │     │
│  │  PUT    /api/tasks/:id → Update task         │     │
│  │  DELETE /api/tasks/:id → Delete task         │     │
│  └─────────────────┬────────────────────────────┘     │
│                    │                                    │
│  ┌─────────────────▼────────────────────────────┐     │
│  │         NODE.JS (Runtime)                     │     │
│  │  - Runs JavaScript on server                 │     │
│  │  - Handles async operations                  │     │
│  │  - Event-driven                              │     │
│  └─────────────────┬────────────────────────────┘     │
└────────────────────┼────────────────────────────────────┘
                     │
                     │ Database Queries
                     │ (CRUD Operations)
                     │
┌────────────────────▼────────────────────────────────────┐
│              DATABASE (MongoDB)                         │
│                                                         │
│  Collections:                                          │
│  ┌─────────────────────────────────┐                  │
│  │  Tasks                          │                  │
│  │  ├─ {id: 1, task: "Study"}     │                  │
│  │  ├─ {id: 2, task: "Code"}      │                  │
│  │  └─ {id: 3, task: "Sleep"}     │                  │
│  └─────────────────────────────────┘                  │
│                                                         │
│  ┌─────────────────────────────────┐                  │
│  │  Users                          │                  │
│  │  ├─ {id: 1, name: "Alice"}     │                  │
│  │  └─ {id: 2, name: "Bob"}       │                  │
│  └─────────────────────────────────┘                  │
└─────────────────────────────────────────────────────────┘
```

**Data Flow Example:**

```
Step 1: User adds a task in React
  ↓
Step 2: React sends POST request to Express
  Request: POST /api/tasks
  Body: { task: "Buy groceries" }
  ↓
Step 3: Express receives request
  ↓
Step 4: Express (running on Node.js) processes
  ↓
Step 5: Express saves to MongoDB
  db.tasks.insertOne({ task: "Buy groceries" })
  ↓
Step 6: MongoDB saves and returns confirmation
  ↓
Step 7: Express sends response to React
  Response: { id: 4, task: "Buy groceries" }
  ↓
Step 8: React updates UI and shows new task
```

**Remember:** React → Express → Node.js → MongoDB → Back to React

---

## Part 2: Node.js

### 1. Explain Node.js modules and require/export (5 marks)

**Answer:**

**Module** = A file with reusable code that can be imported into other files.

**Why Use Modules?**

- Organize code into separate files
- Reuse code in multiple places
- Keep code clean and manageable

**Two Ways to Use Modules:**

**1. CommonJS (Traditional - Node.js)**

```javascript
// Export
module.exports = ...

// Import
require('...')
```

**2. ES6 Modules (Modern)**

```javascript
// Export
export default ...

// Import
import ... from '...'
```

**CommonJS Example:**

**File 1: mathUtils.js (Exports functions)**

```javascript
// Define functions
function add(a, b) {
  return a + b
}

function subtract(a, b) {
  return a - b
}

function multiply(a, b) {
  return a * b
}

// Export single function
module.exports = add

// OR export multiple functions
module.exports = {
  add: add,
  subtract: subtract,
  multiply: multiply,
}

// OR shorthand
module.exports = { add, subtract, multiply }
```

**File 2: main.js (Imports and uses)**

```javascript
// Import the module
const mathUtils = require("./mathUtils")

// Use the functions
console.log(mathUtils.add(5, 3)) // 8
console.log(mathUtils.subtract(10, 4)) // 6
console.log(mathUtils.multiply(3, 4)) // 12
```

**Different Export Styles:**

**Style 1: Export Object**

```javascript
// calculator.js
module.exports = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b,
}

// main.js
const calc = require("./calculator")
console.log(calc.add(5, 3))
```

**Style 2: Export Single Function**

```javascript
// greet.js
function greet(name) {
  return `Hello, ${name}!`
}

module.exports = greet

// main.js
const greet = require("./greet")
console.log(greet("John")) // Hello, John!
```

**Style 3: Add to exports directly**

```javascript
// utils.js
exports.add = (a, b) => a + b
exports.subtract = (a, b) => a - b

// main.js
const utils = require("./utils")
console.log(utils.add(5, 3))
```

**Complete Example:**

**student.js**

```javascript
// Student class
class Student {
  constructor(name, age, course) {
    this.name = name
    this.age = age
    this.course = course
  }

  introduce() {
    return `Hi, I'm ${this.name}, ${this.age} years old, studying ${this.course}`
  }
}

// Export the class
module.exports = Student
```

**main.js**

```javascript
// Import Student class
const Student = require("./student")

// Create students
const student1 = new Student("Alice", 20, "Computer Science")
const student2 = new Student("Bob", 22, "Mathematics")

// Use the class
console.log(student1.introduce())
// Hi, I'm Alice, 20 years old, studying Computer Science

console.log(student2.introduce())
// Hi, I'm Bob, 22 years old, studying Mathematics
```

**Built-in Node.js Modules:**

```javascript
// No ./ needed for built-in modules
const fs = require("fs") // File system
const http = require("http") // HTTP server
const path = require("path") // Path utilities
```

**Remember:**

- `module.exports` = Export from file
- `require()` = Import into file
- Use `./` for local files

---

### 2. Odd/Even Number Generator with CommonJS (10 marks)

**Answer:**

**Project Structure:**

```
project/
├── numberUtils.js  (Module - exports functions)
└── main.js         (Main file - uses module)
```

**numberUtils.js (Module)**

```javascript
// Function to generate odd numbers
function generateOddNumbers(limit) {
  const oddNumbers = []

  for (let i = 1; i <= limit; i++) {
    if (i % 2 !== 0) {
      // i is odd if remainder is not 0
      oddNumbers.push(i)
    }
  }

  return oddNumbers
}

// Function to generate even numbers
function generateEvenNumbers(limit) {
  const evenNumbers = []

  for (let i = 1; i <= limit; i++) {
    if (i % 2 === 0) {
      // i is even if remainder is 0
      evenNumbers.push(i)
    }
  }

  return evenNumbers
}

// Function to check if number is odd
function isOdd(number) {
  return number % 2 !== 0
}

// Function to check if number is even
function isEven(number) {
  return number % 2 === 0
}

// Export all functions
module.exports = {
  generateOddNumbers,
  generateEvenNumbers,
  isOdd,
  isEven,
}
```

**main.js (Main File)**

```javascript
// Import the module
const numberUtils = require("./numberUtils")

// Set limit
const limit = 20

console.log("=== Number Generator ===\n")

// Generate odd numbers
const oddNumbers = numberUtils.generateOddNumbers(limit)
console.log(`Odd numbers from 1 to ${limit}:`)
console.log(oddNumbers.join(", "))
console.log(`Count: ${oddNumbers.length}\n`)

// Generate even numbers
const evenNumbers = numberUtils.generateEvenNumbers(limit)
console.log(`Even numbers from 1 to ${limit}:`)
console.log(evenNumbers.join(", "))
console.log(`Count: ${evenNumbers.length}\n`)

// Test individual numbers
const testNumber = 15
console.log(`Is ${testNumber} odd? ${numberUtils.isOdd(testNumber)}`)
console.log(`Is ${testNumber} even? ${numberUtils.isEven(testNumber)}`)
```

**Output:**

```
=== Number Generator ===

Odd numbers from 1 to 20:
1, 3, 5, 7, 9, 11, 13, 15, 17, 19
Count: 10

Even numbers from 1 to 20:
2, 4, 6, 8, 10, 12, 14, 16, 18, 20
Count: 10

Is 15 odd? true
Is 15 even? false
```

**Alternative with Separate Files:**

**oddNumbers.js**

```javascript
function generateOdd(limit) {
  const result = []
  for (let i = 1; i <= limit; i += 2) {
    // Start at 1, increment by 2
    result.push(i)
  }
  return result
}

module.exports = generateOdd
```

**evenNumbers.js**

```javascript
function generateEven(limit) {
  const result = []
  for (let i = 2; i <= limit; i += 2) {
    // Start at 2, increment by 2
    result.push(i)
  }
  return result
}

module.exports = generateEven
```

**main.js**

```javascript
const generateOdd = require("./oddNumbers")
const generateEven = require("./evenNumbers")

const limit = 30

console.log("Odd:", generateOdd(limit))
console.log("Even:", generateEven(limit))
```

**How to Run:**

```bash
# In terminal
node main.js
```

**Key Concepts:**

- ✅ Modular separation (each file has one purpose)
- ✅ Reusable functions
- ✅ Clean exports and imports
- ✅ Easy to test and maintain

---

### 3. Math Functions Module (10 marks)

**Answer:**

**mathFunctions.js**

```javascript
// Factorial function
function factorial(n) {
  if (n === 0 || n === 1) {
    return 1
  }

  let result = 1
  for (let i = 2; i <= n; i++) {
    result *= i
  }
  return result
}

// Square function
function square(n) {
  return n * n
}

// Cube function
function cube(n) {
  return n * n * n
}

// Power function (bonus)
function power(base, exponent) {
  return Math.pow(base, exponent)
}

// Square root function (bonus)
function squareRoot(n) {
  return Math.sqrt(n)
}

// Export all functions
module.exports = {
  factorial,
  square,
  cube,
  power,
  squareRoot,
}
```

**main.js**

```javascript
// Import math functions
const math = require("./mathFunctions")

console.log("=== Math Functions Demo ===\n")

// Sample inputs
const numbers = [5, 10, 3, 7]

// Test each function
numbers.forEach((num) => {
  console.log(`Number: ${num}`)
  console.log(`  Factorial: ${math.factorial(num)}`)
  console.log(`  Square: ${math.square(num)}`)
  console.log(`  Cube: ${math.cube(num)}`)
  console.log(`  Square Root: ${math.squareRoot(num).toFixed(2)}`)
  console.log("")
})

// Additional examples
console.log("=== Additional Examples ===\n")
console.log(`5! = ${math.factorial(5)}`)
console.log(`10² = ${math.square(10)}`)
console.log(`3³ = ${math.cube(3)}`)
console.log(`2⁸ = ${math.power(2, 8)}`)
console.log(`√16 = ${math.squareRoot(16)}`)
```

**Output:**

```
=== Math Functions Demo ===

Number: 5
  Factorial: 120
  Square: 25
  Cube: 125
  Square Root: 2.24

Number: 10
  Factorial: 3628800
  Square: 100
  Cube: 1000
  Square Root: 3.16

Number: 3
  Factorial: 6
  Square: 9
  Cube: 27
  Square Root: 1.73

Number: 7
  Factorial: 5040
  Square: 49
  Cube: 343
  Square Root: 2.65

=== Additional Examples ===

5! = 120
10² = 100
3³ = 27
2⁸ = 256
√16 = 4
```

**With User Input:**

**main.js (with readline)**

```javascript
const math = require("./mathFunctions")
const readline = require("readline")

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
})

function calculate() {
  rl.question("Enter a number: ", (input) => {
    const num = parseInt(input)

    if (isNaN(num)) {
      console.log("Please enter a valid number!")
      rl.close()
      return
    }

    console.log("\n=== Results ===")
    console.log(`Factorial of ${num}: ${math.factorial(num)}`)
    console.log(`Square of ${num}: ${math.square(num)}`)
    console.log(`Cube of ${num}: ${math.cube(num)}`)
    console.log(`Square root of ${num}: ${math.squareRoot(num).toFixed(2)}`)

    rl.close()
  })
}

calculate()
```

**Remember:** Modular code = Reusable and organized!

---

### 4. Calculator Module with User Input (10 marks)

**Answer:**

**calculator.js (Module)**

```javascript
// Addition
function add(a, b) {
  return a + b
}

// Subtraction
function subtract(a, b) {
  return a - b
}

// Multiplication
function multiply(a, b) {
  return a * b
}

// Division
function divide(a, b) {
  if (b === 0) {
    return "Error: Cannot divide by zero"
  }
  return a / b
}

// Modulus (remainder)
function modulus(a, b) {
  return a % b
}

// Power
function power(a, b) {
  return Math.pow(a, b)
}

// Export all functions
module.exports = {
  add,
  subtract,
  multiply,
  divide,
  modulus,
  power,
}
```

**main.js (User Input)**

```javascript
const calc = require("./calculator")
const readline = require("readline")

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
})

console.log("=== Simple Calculator ===\n")
console.log("Operations:")
console.log("1. Add (+)")
console.log("2. Subtract (-)")
console.log("3. Multiply (*)")
console.log("4. Divide (/)")
console.log("5. Modulus (%)")
console.log("6. Power (^)\n")

// Get first number
rl.question("Enter first number: ", (num1) => {
  const a = parseFloat(num1)

  // Get operation
  rl.question("Enter operation (+, -, *, /, %, ^): ", (operation) => {
    // Get second number
    rl.question("Enter second number: ", (num2) => {
      const b = parseFloat(num2)

      // Calculate based on operation
      let result

      switch (operation) {
        case "+":
          result = calc.add(a, b)
          break
        case "-":
          result = calc.subtract(a, b)
          break
        case "*":
          result = calc.multiply(a, b)
          break
        case "/":
          result = calc.divide(a, b)
          break
        case "%":
          result = calc.modulus(a, b)
          break
        case "^":
          result = calc.power(a, b)
          break
        default:
          result = "Invalid operation!"
      }

      // Display result
      console.log(`\n${a} ${operation} ${b} = ${result}`)

      rl.close()
    })
  })
})
```

**Interactive Session:**

```
=== Simple Calculator ===

Operations:
1. Add (+)
2. Subtract (-)
3. Multiply (*)
4. Divide (/)
5. Modulus (%)
6. Power (^)

Enter first number: 10
Enter operation (+, -, *, /, %, ^): +
Enter second number: 5

10 + 5 = 15
```

**Alternative: Menu-Based Calculator:**

**main.js (Advanced)**

```javascript
const calc = require("./calculator")
const readline = require("readline")

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
})

function showMenu() {
  console.log("\n=== Calculator Menu ===")
  console.log("1. Addition")
  console.log("2. Subtraction")
  console.log("3. Multiplication")
  console.log("4. Division")
  console.log("5. Modulus")
  console.log("6. Power")
  console.log("7. Exit\n")
}

function calculate() {
  showMenu()

  rl.question("Choose operation (1-7): ", (choice) => {
    if (choice === "7") {
      console.log("Goodbye!")
      rl.close()
      return
    }

    rl.question("Enter first number: ", (num1) => {
      rl.question("Enter second number: ", (num2) => {
        const a = parseFloat(num1)
        const b = parseFloat(num2)
        let result

        switch (choice) {
          case "1":
            result = calc.add(a, b)
            console.log(`Result: ${a} + ${b} = ${result}`)
            break
          case "2":
            result = calc.subtract(a, b)
            console.log(`Result: ${a} - ${b} = ${result}`)
            break
          case "3":
            result = calc.multiply(a, b)
            console.log(`Result: ${a} × ${b} = ${result}`)
            break
          case "4":
            result = calc.divide(a, b)
            console.log(`Result: ${a} ÷ ${b} = ${result}`)
            break
          case "5":
            result = calc.modulus(a, b)
            console.log(`Result: ${a} % ${b} = ${result}`)
            break
          case "6":
            result = calc.power(a, b)
            console.log(`Result: ${a}^${b} = ${result}`)
            break
          default:
            console.log("Invalid choice!")
        }

        // Continue or exit
        rl.question("\nPerform another calculation? (y/n): ", (answer) => {
          if (answer.toLowerCase() === "y") {
            calculate()
          } else {
            console.log("Goodbye!")
            rl.close()
          }
        })
      })
    })
  })
}

// Start calculator
calculate()
```

**How to Run:**

```bash
node main.js
```

**Key Concepts:**

- ✅ Separate calculator logic (module)
- ✅ User input handling
- ✅ Error handling (divide by zero)
- ✅ Interactive menu

---

### 5. Prime Number Generator (10 marks)

**Answer:**

**primeGenerator.js (Module)**

```javascript
// Function to check if a number is prime
function isPrime(num) {
  if (num <= 1) return false // 0 and 1 are not prime
  if (num === 2) return true // 2 is prime
  if (num % 2 === 0) return false // Even numbers not prime

  // Check odd divisors up to square root
  for (let i = 3; i <= Math.sqrt(num); i += 2) {
    if (num % i === 0) {
      return false // Found a divisor, not prime
    }
  }

  return true // No divisors found, is prime
}

// Function to generate all prime numbers up to limit
function generatePrimes(limit) {
  const primes = []

  for (let i = 2; i <= limit; i++) {
    if (isPrime(i)) {
      primes.push(i)
    }
  }

  return primes
}

// Function to get count of primes
function countPrimes(limit) {
  return generatePrimes(limit).length
}

// Function to get nth prime
function getNthPrime(n) {
  let count = 0
  let num = 2

  while (count < n) {
    if (isPrime(num)) {
      count++
      if (count === n) {
        return num
      }
    }
    num++
  }
}

// Export functions
module.exports = {
  generatePrimes,
  isPrime,
  countPrimes,
  getNthPrime,
}
```

**main.js (Display Output)**

```javascript
const primes = require("./primeGenerator")

console.log("=== Prime Number Generator ===\n")

// Test different limits
const limits = [10, 20, 50, 100]

limits.forEach((limit) => {
  const primeNumbers = primes.generatePrimes(limit)

  console.log(`Prime numbers up to ${limit}:`)
  console.log(primeNumbers.join(", "))
  console.log(`Count: ${primeNumbers.length}\n`)
})

// Additional examples
console.log("=== Additional Examples ===\n")

// Test individual numbers
const testNumbers = [17, 20, 23, 30]
testNumbers.forEach((num) => {
  console.log(`Is ${num} prime? ${primes.isPrime(num)}`)
})

// Get specific primes
console.log(`\n10th prime number: ${primes.getNthPrime(10)}`)
console.log(`25th prime number: ${primes.getNthPrime(25)}`)
```

**Output:**

```
=== Prime Number Generator ===

Prime numbers up to 10:
2, 3, 5, 7
Count: 4

Prime numbers up to 20:
2, 3, 5, 7, 11, 13, 17, 19
Count: 8

Prime numbers up to 50:
2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47
Count: 15

Prime numbers up to 100:
2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97
Count: 25

=== Additional Examples ===

Is 17 prime? true
Is 20 prime? false
Is 23 prime? true
Is 30 prime? false

10th prime number: 29
25th prime number: 97
```

**With User Input:**

**main.js (Interactive)**

```javascript
const primes = require("./primeGenerator")
const readline = require("readline")

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
})

console.log("=== Prime Number Generator ===\n")

rl.question("Enter limit: ", (input) => {
  const limit = parseInt(input)

  if (isNaN(limit) || limit < 2) {
    console.log("Please enter a valid number >= 2")
    rl.close()
    return
  }

  // Generate primes
  const primeNumbers = primes.generatePrimes(limit)

  console.log(`\nPrime numbers from 2 to ${limit}:`)
  console.log(primeNumbers.join(", "))
  console.log(`\nTotal primes found: ${primeNumbers.length}`)

  // Show first and last
  if (primeNumbers.length > 0) {
    console.log(`First prime: ${primeNumbers[0]}`)
    console.log(`Last prime: ${primeNumbers[primeNumbers.length - 1]}`)
  }

  rl.close()
})
```

**Explanation - How isPrime Works:**

```javascript
// Example: Is 17 prime?
isPrime(17)
  ↓
Check if <= 1? No
  ↓
Check if === 2? No
  ↓
Check if even (17 % 2 === 0)? No
  ↓
Check divisors from 3 to √17 (≈4.12)
  17 % 3 = 2 (not divisible)
  (loop ends, no more divisors to check)
  ↓
No divisors found → Return true (17 is prime!)
```

**Remember:**

- Prime = Only divisible by 1 and itself
- Check divisibility up to √n for efficiency

---

(Continuing in next response due to length...)

# Unit I - Full Stack Development Q&A Guide Part 2

## Part 3: Express.js

### 1. Compare Express.js with Node.js HTTP module (6 marks)

**Answer:**

**Node.js HTTP Module** = Built-in module for creating servers (basic, more code needed)
**Express.js** = Framework built on top of HTTP module (easier, less code)

**Comparison:**

| Feature        | Node.js HTTP | Express.js         |
| -------------- | ------------ | ------------------ |
| **Setup**      | More code    | Less code          |
| **Routing**    | Manual       | Built-in           |
| **Middleware** | Manual       | Built-in           |
| **Parsing**    | Manual       | Easy (body-parser) |
| **Learning**   | Harder       | Easier             |

**Example Comparison:**

**Node.js HTTP Module:**

```javascript
const http = require("http")
const url = require("url")

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url, true)
  const path = parsedUrl.pathname

  // Manual routing
  if (path === "/" && req.method === "GET") {
    res.writeHead(200, { "Content-Type": "text/plain" })
    res.end("Home Page")
  } else if (path === "/about" && req.method === "GET") {
    res.writeHead(200, { "Content-Type": "text/plain" })
    res.end("About Page")
  } else if (path === "/users" && req.method === "POST") {
    // Manual body parsing
    let body = ""
    req.on("data", (chunk) => {
      body += chunk.toString()
    })
    req.on("end", () => {
      const data = JSON.parse(body)
      res.writeHead(200, { "Content-Type": "application/json" })
      res.end(JSON.stringify({ message: "User created", data }))
    })
  } else {
    res.writeHead(404, { "Content-Type": "text/plain" })
    res.end("Not Found")
  }
})

server.listen(3000, () => {
  console.log("Server running on port 3000")
})
```

**Express.js (Same functionality):**

```javascript
const express = require("express")
const app = express()

// Built-in body parsing
app.use(express.json())

// Easy routing
app.get("/", (req, res) => {
  res.send("Home Page")
})

app.get("/about", (req, res) => {
  res.send("About Page")
})

app.post("/users", (req, res) => {
  const data = req.body // Already parsed!
  res.json({ message: "User created", data })
})

// 404 handler
app.use((req, res) => {
  res.status(404).send("Not Found")
})

app.listen(3000, () => {
  console.log("Server running on port 3000")
})
```

**Key Differences:**

**1. Routing:**

```javascript
// HTTP - Manual if-else
if (path === '/users' && req.method === 'GET') { ... }
if (path === '/users' && req.method === 'POST') { ... }

// Express - Clean and simple
app.get('/users', (req, res) => { ... });
app.post('/users', (req, res) => { ... });
```

**2. Middleware:**

```javascript
// HTTP - Manual implementation
req.on("data", (chunk) => {
  body += chunk
})

// Express - Built-in
app.use(express.json()) // Automatically parses JSON
```

**3. Response Handling:**

```javascript
// HTTP - Verbose
res.writeHead(200, { "Content-Type": "application/json" })
res.end(JSON.stringify(data))

// Express - Simple
res.json(data) // Sets headers and sends automatically
```

**When to Use:**

- **HTTP Module**: Learning, simple projects, low-level control
- **Express.js**: Production apps, complex routing, middleware needed

**Remember:** Express makes Node.js development much easier!

---

### 2. Five properties/methods of Express app object (5 marks)

**Answer:**

**1. app.get(path, handler)**

```javascript
// Handle GET requests
app.get("/users", (req, res) => {
  res.send("Get all users")
})

app.get("/users/:id", (req, res) => {
  res.send(`Get user ${req.params.id}`)
})
```

**2. app.post(path, handler)**

```javascript
// Handle POST requests
app.post("/users", (req, res) => {
  const newUser = req.body
  res.json({ message: "User created", user: newUser })
})
```

**3. app.use(middleware)**

```javascript
// Apply middleware
app.use(express.json()) // Parse JSON

app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`)
  next()
})

// Mount at specific path
app.use("/api", apiRoutes)
```

**4. app.listen(port, callback)**

```javascript
// Start server
app.listen(3000, () => {
  console.log("Server running on port 3000")
})

// With host
app.listen(3000, "localhost", () => {
  console.log("Server started")
})
```

**5. app.set(name, value) / app.get(name)**

```javascript
// Set application settings
app.set("port", 3000)
app.set("view engine", "ejs")
app.set("views", "./views")

// Get settings
const port = app.get("port")
console.log("Port:", port) // 3000
```

**Complete Example:**

```javascript
const express = require("express")
const app = express()

// 1. app.use() - Middleware
app.use(express.json())
app.use(express.static("public"))

// 2. app.set() - Settings
app.set("port", process.env.PORT || 3000)

// 3. app.get() - GET routes
app.get("/", (req, res) => {
  res.send("Home Page")
})

app.get("/about", (req, res) => {
  res.send("About Page")
})

// 4. app.post() - POST routes
app.post("/users", (req, res) => {
  res.json({ message: "User created" })
})

// 5. app.listen() - Start server
const port = app.get("port")
app.listen(port, () => {
  console.log(`Server running on port ${port}`)
})
```

**Other Useful Methods:**

```javascript
app.put("/users/:id", handler) // Update
app.delete("/users/:id", handler) // Delete
app.all("/secret", handler) // All HTTP methods
app
  .route("/users") // Chain routes
  .get(handler)
  .post(handler)
```

---

### 3. Purpose of req.params (5 marks)

**Answer:**

**req.params** = Object containing route parameters (dynamic parts of URL).

**What are Route Parameters?**
Parts of URL that can change (marked with `:`)

**Syntax:**

```javascript
app.get("/users/:id", (req, res) => {
  const userId = req.params.id
  res.send(`User ID: ${userId}`)
})
```

**Examples:**

**Example 1: Single Parameter**

```javascript
// Route definition
app.get("/users/:id", (req, res) => {
  const id = req.params.id
  res.send(`Fetching user with ID: ${id}`)
})

// URL: /users/123
// req.params = { id: '123' }
// Output: "Fetching user with ID: 123"
```

**Example 2: Multiple Parameters**

```javascript
// Route definition
app.get("/users/:userId/posts/:postId", (req, res) => {
  const { userId, postId } = req.params
  res.send(`User ${userId}, Post ${postId}`)
})

// URL: /users/5/posts/10
// req.params = { userId: '5', postId: '10' }
// Output: "User 5, Post 10"
```

**Example 3: Real-World Usage**

```javascript
const express = require("express")
const app = express()

// Sample data
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 3, name: "Charlie" },
]

// Get user by ID
app.get("/users/:id", (req, res) => {
  const userId = parseInt(req.params.id)

  const user = users.find((u) => u.id === userId)

  if (user) {
    res.json(user)
  } else {
    res.status(404).json({ error: "User not found" })
  }
})

// URL: /users/2
// Response: { id: 2, name: 'Bob' }
```

**Example 4: Product Route**

```javascript
app.get("/products/:category/:productId", (req, res) => {
  const { category, productId } = req.params

  res.json({
    message: "Product found",
    category: category,
    productId: productId,
  })
})

// URL: /products/electronics/laptop-123
// req.params = { category: 'electronics', productId: 'laptop-123' }
```

**Important Notes:**

```javascript
// Always strings!
app.get("/users/:id", (req, res) => {
  console.log(typeof req.params.id) // 'string'

  // Convert to number if needed
  const id = parseInt(req.params.id)
  console.log(typeof id) // 'number'
})
```

**req.params vs req.query:**

```javascript
// req.params - Part of URL path
app.get("/users/:id", (req, res) => {
  // URL: /users/5
  // req.params.id = '5'
})

// req.query - After ? in URL
app.get("/users", (req, res) => {
  // URL: /users?age=25&city=NYC
  // req.query = { age: '25', city: 'NYC' }
})
```

**Remember:** req.params = Dynamic parts of URL (marked with `:`)

---

### 4. Role of app.use() in Express middleware (4 marks)

**Answer:**

**app.use()** = Function to add middleware to Express application.

**What is Middleware?**
Functions that run **between** receiving request and sending response.

```
Request → Middleware 1 → Middleware 2 → Route Handler → Response
```

**Basic Syntax:**

```javascript
app.use(middlewareFunction)

app.use((req, res, next) => {
  // Do something
  next() // Pass to next middleware
})
```

**Examples:**

**1. Global Middleware (runs for all routes)**

```javascript
// Logger middleware
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date()}`)
  next() // IMPORTANT: Call next()
})

// All routes below will use this logger
app.get("/", (req, res) => {
  res.send("Home")
})

app.get("/about", (req, res) => {
  res.send("About")
})
```

**2. Built-in Middleware**

```javascript
// Parse JSON bodies
app.use(express.json())

// Parse URL-encoded bodies
app.use(express.urlencoded({ extended: true }))

// Serve static files
app.use(express.static("public"))
```

**3. Path-Specific Middleware**

```javascript
// Only for /api routes
app.use("/api", (req, res, next) => {
  console.log("API request")
  next()
})

app.get("/api/users", (req, res) => {
  res.json({ users: [] })
})
```

**4. Third-Party Middleware**

```javascript
const cors = require("cors")
const morgan = require("morgan")

app.use(cors()) // Enable CORS
app.use(morgan("dev")) // HTTP request logger
```

**Complete Example:**

```javascript
const express = require("express")
const app = express()

// 1. Built-in middleware
app.use(express.json())

// 2. Custom logger
app.use((req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`)
  next()
})

// 3. Authentication check
app.use("/admin", (req, res, next) => {
  const isAuthenticated = true // Check auth token

  if (isAuthenticated) {
    next() // Allow access
  } else {
    res.status(401).send("Unauthorized")
  }
})

// Routes
app.get("/", (req, res) => {
  res.send("Public page")
})

app.get("/admin", (req, res) => {
  res.send("Admin page - Protected")
})

app.listen(3000)
```

**Execution Order:**

```javascript
app.use(middleware1) // Runs first
app.use(middleware2) // Runs second
app.get("/", handler) // Runs last

// Request flow:
// Request → middleware1 → middleware2 → handler → Response
```

**Remember:**

- `app.use()` = Add middleware
- Always call `next()` to continue
- Order matters!

---

### 5. Explain res.redirect() with example (4 marks)

**Answer:**

**res.redirect()** = Redirect client to a different URL.

**Syntax:**

```javascript
res.redirect("/new-url")
res.redirect(301, "/new-url") // With status code
res.redirect("https://google.com") // External URL
```

**Examples:**

**Example 1: After Form Submission**

```javascript
// POST form handler
app.post("/login", (req, res) => {
  const { username, password } = req.body

  if (username === "admin" && password === "pass123") {
    // Successful login - redirect to dashboard
    res.redirect("/dashboard")
  } else {
    // Failed login - redirect back to login
    res.redirect("/login?error=invalid")
  }
})

app.get("/dashboard", (req, res) => {
  res.send("Welcome to Dashboard!")
})
```

**Example 2: Old URL to New URL**

```javascript
// Permanent redirect (301)
app.get("/old-page", (req, res) => {
  res.redirect(301, "/new-page")
})

app.get("/new-page", (req, res) => {
  res.send("This is the new page")
})
```

**Example 3: After Creating Resource**

```javascript
app.post("/users", (req, res) => {
  const newUser = req.body

  // Save to database
  const user = saveUser(newUser)

  // Redirect to user profile
  res.redirect(`/users/${user.id}`)
})

app.get("/users/:id", (req, res) => {
  res.send(`User profile ${req.params.id}`)
})
```

**Example 4: External Redirect**

```javascript
app.get("/google", (req, res) => {
  res.redirect("https://www.google.com")
})
```

**Status Codes:**

```javascript
res.redirect("/page") // 302 (Temporary)
res.redirect(301, "/page") // 301 (Permanent)
res.redirect(302, "/page") // 302 (Temporary)
res.redirect(307, "/page") // 307 (Temporary, preserve method)
```

**Complete Example:**

```javascript
const express = require("express")
const app = express()

app.use(express.json())

// Home page
app.get("/", (req, res) => {
  res.send(`
    <h1>Home</h1>
    <form action="/search" method="GET">
      <input name="q" placeholder="Search">
      <button>Search</button>
    </form>
  `)
})

// Search redirect
app.get("/search", (req, res) => {
  const query = req.query.q

  if (!query) {
    // No search term - redirect to home
    res.redirect("/")
  } else {
    // Redirect to Google search
    res.redirect(`https://www.google.com/search?q=${query}`)
  }
})

// Old URL redirect
app.get("/old-about", (req, res) => {
  res.redirect(301, "/about") // Permanent
})

app.get("/about", (req, res) => {
  res.send("About page")
})

app.listen(3000)
```

**Remember:** res.redirect() sends user to different URL

---

### 6. Function of req.body (4 marks)

**Answer:**

**req.body** = Contains data sent in the request body (from forms, AJAX, etc.)

**Important:** Need middleware to parse body!

```javascript
app.use(express.json()) // For JSON
app.use(express.urlencoded({ extended: true })) // For forms
```

**Examples:**

**Example 1: JSON Data (API)**

```javascript
const express = require("express")
const app = express()

// MUST have this middleware
app.use(express.json())

app.post("/users", (req, res) => {
  const userData = req.body

  console.log(userData)
  // { name: 'John', email: 'john@example.com' }

  res.json({
    message: "User created",
    user: userData,
  })
})

// Client sends:
// POST /users
// Content-Type: application/json
// Body: { "name": "John", "email": "john@example.com" }
```

**Example 2: Form Data**

```javascript
app.use(express.urlencoded({ extended: true }))

app.post("/login", (req, res) => {
  const { username, password } = req.body

  console.log("Username:", username)
  console.log("Password:", password)

  res.send(`Welcome ${username}!`)
})

// HTML Form:
// <form action="/login" method="POST">
//   <input name="username">
//   <input name="password" type="password">
//   <button>Login</button>
// </form>
```

**Example 3: Complete CRUD**

```javascript
const express = require("express")
const app = express()

app.use(express.json())

let users = []

// Create user
app.post("/users", (req, res) => {
  const { name, email, age } = req.body

  // Validation
  if (!name || !email) {
    return res.status(400).json({ error: "Name and email required" })
  }

  const newUser = {
    id: users.length + 1,
    name,
    email,
    age: age || 0,
  }

  users.push(newUser)
  res.status(201).json(newUser)
})

// Update user
app.put("/users/:id", (req, res) => {
  const id = parseInt(req.params.id)
  const updates = req.body

  const userIndex = users.findIndex((u) => u.id === id)

  if (userIndex === -1) {
    return res.status(404).json({ error: "User not found" })
  }

  users[userIndex] = { ...users[userIndex], ...updates }
  res.json(users[userIndex])
})

app.listen(3000)
```

**What req.body contains:**

```javascript
// POST /users
// Body: { "name": "Alice", "email": "alice@example.com" }

app.post("/users", (req, res) => {
  console.log(req.body)
  // Output: { name: 'Alice', email: 'alice@example.com' }

  console.log(req.body.name) // Alice
  console.log(req.body.email) // alice@example.com
})
```

**Remember:**

- req.body = Data sent in POST/PUT request
- Need `express.json()` middleware!

---

### 7. Four external middlewares for Express (4 marks)

**Answer:**

**1. CORS (Cross-Origin Resource Sharing)**

```javascript
const cors = require("cors")

// Allow all origins
app.use(cors())

// Custom configuration
app.use(
  cors({
    origin: "http://localhost:3000",
    methods: ["GET", "POST"],
    credentials: true,
  }),
)

// Allows front-end on different domain to access API
```

**2. Morgan (HTTP Request Logger)**

```javascript
const morgan = require("morgan")

// Development logging
app.use(morgan("dev"))
// Output: GET /users 200 15.234 ms

// Combined format (production)
app.use(morgan("combined"))

// Logs all HTTP requests
```

**3. Helmet (Security Headers)**

```javascript
const helmet = require("helmet")

app.use(helmet())

// Sets secure HTTP headers:
// - X-Content-Type-Options
// - X-Frame-Options
// - Strict-Transport-Security
// Protects from common vulnerabilities
```

**4. Body-Parser (Parse Request Bodies)**

```javascript
const bodyParser = require("body-parser")

// Parse JSON
app.use(bodyParser.json())

// Parse URL-encoded (forms)
app.use(bodyParser.urlencoded({ extended: true }))

// Note: Express now has built-in body parsing
// express.json() and express.urlencoded()
```

**Complete Example:**

```javascript
const express = require("express")
const cors = require("cors")
const morgan = require("morgan")
const helmet = require("helmet")

const app = express()

// 1. Security
app.use(helmet())

// 2. CORS
app.use(cors())

// 3. Logging
app.use(morgan("dev"))

// 4. Body parsing
app.use(express.json())
app.use(express.urlencoded({ extended: true }))

// Routes
app.get("/api/users", (req, res) => {
  res.json({ users: [] })
})

app.listen(3000, () => {
  console.log("Server running with middleware")
})
```

**Other Popular Middleware:**

```javascript
// Express Session
const session = require("express-session")
app.use(session({ secret: "key", resave: false }))

// Compression
const compression = require("compression")
app.use(compression())

// Cookie Parser
const cookieParser = require("cookie-parser")
app.use(cookieParser())

// Express Validator
const { body, validationResult } = require("express-validator")
```

---

### 8. Handle POST request and return JSON (6 marks)

**Answer:**

```javascript
const express = require("express")
const app = express()

// REQUIRED: Parse JSON bodies
app.use(express.json())

// Sample data store
let users = [
  { id: 1, name: "Alice", email: "alice@example.com" },
  { id: 2, name: "Bob", email: "bob@example.com" },
]

// POST route to create user
app.post("/users", (req, res) => {
  // Get data from request body
  const { name, email } = req.body

  // Validate data
  if (!name || !email) {
    return res.status(400).json({
      success: false,
      error: "Name and email are required",
    })
  }

  // Create new user
  const newUser = {
    id: users.length + 1,
    name: name,
    email: email,
    createdAt: new Date(),
  }

  // Add to array
  users.push(newUser)

  // Send JSON response
  res.status(201).json({
    success: true,
    message: "User created successfully",
    user: newUser,
  })
})

// GET all users
app.get("/users", (req, res) => {
  res.json({
    success: true,
    count: users.length,
    users: users,
  })
})

// Start server
app.listen(3000, () => {
  console.log("Server running on port 3000")
})
```

**Testing with Different Tools:**

**1. Using Postman/Thunder Client:**

```
POST http://localhost:3000/users
Headers:
  Content-Type: application/json
Body (JSON):
{
  "name": "Charlie",
  "email": "charlie@example.com"
}

Response:
{
  "success": true,
  "message": "User created successfully",
  "user": {
    "id": 3,
    "name": "Charlie",
    "email": "charlie@example.com",
    "createdAt": "2024-02-09T10:30:00.000Z"
  }
}
```

**2. Using curl:**

```bash
curl -X POST http://localhost:3000/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Charlie","email":"charlie@example.com"}'
```

**3. Using JavaScript fetch:**

```javascript
fetch("http://localhost:3000/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    name: "Charlie",
    email: "charlie@example.com",
  }),
})
  .then((res) => res.json())
  .then((data) => console.log(data))
```

**Complete Example with Validation:**

```javascript
const express = require("express")
const app = express()

app.use(express.json())

let posts = []

app.post("/posts", (req, res) => {
  const { title, content, author } = req.body

  // Validate all fields
  const errors = []

  if (!title) errors.push("Title is required")
  if (!content) errors.push("Content is required")
  if (!author) errors.push("Author is required")

  if (title && title.length < 5) {
    errors.push("Title must be at least 5 characters")
  }

  if (errors.length > 0) {
    return res.status(400).json({
      success: false,
      errors: errors,
    })
  }

  // Create post
  const newPost = {
    id: posts.length + 1,
    title,
    content,
    author,
    createdAt: new Date(),
    likes: 0,
  }

  posts.push(newPost)

  res.status(201).json({
    success: true,
    message: "Post created",
    post: newPost,
  })
})

app.listen(3000)
```

**Remember:**

- Use `express.json()` middleware
- Validate input data
- Return appropriate status codes
- Send JSON response with `res.json()`

---

### 9. Differentiate res.send() vs res.json() (6 marks)

**Answer:**

| Feature          | res.send()             | res.json()              |
| ---------------- | ---------------------- | ----------------------- |
| **Content Type** | Detects automatically  | Always JSON             |
| **Input**        | String, Buffer, Object | Object, Array, Number   |
| **Conversion**   | May not convert        | Always converts to JSON |
| **Best For**     | HTML, text             | APIs, data              |

**Examples:**

**res.send():**

```javascript
// Send string
app.get("/hello", (req, res) => {
  res.send("Hello World!")
  // Content-Type: text/html
})

// Send HTML
app.get("/page", (req, res) => {
  res.send("<h1>Welcome</h1><p>This is HTML</p>")
  // Content-Type: text/html
})

// Send object (converts to JSON)
app.get("/user", (req, res) => {
  res.send({ name: "John", age: 25 })
  // Content-Type: application/json
})

// Send array
app.get("/numbers", (req, res) => {
  res.send([1, 2, 3, 4, 5])
  // Content-Type: application/json
})

// Send Buffer
app.get("/buffer", (req, res) => {
  res.send(Buffer.from("Hello"))
  // Content-Type: application/octet-stream
})
```

**res.json():**

```javascript
// Send object (ALWAYS JSON)
app.get("/user", (req, res) => {
  res.json({ name: "John", age: 25 })
  // Content-Type: application/json
  // Body: {"name":"John","age":25}
})

// Send array
app.get("/users", (req, res) => {
  res.json([{ name: "Alice" }, { name: "Bob" }])
  // Content-Type: application/json
})

// Send number
app.get("/count", (req, res) => {
  res.json(42)
  // Content-Type: application/json
  // Body: 42
})

// Send boolean
app.get("/status", (req, res) => {
  res.json(true)
  // Content-Type: application/json
  // Body: true
})

// Send null
app.get("/data", (req, res) => {
  res.json(null)
  // Content-Type: application/json
  // Body: null
})
```

**Side-by-Side Comparison:**

```javascript
const express = require("express")
const app = express()

const user = { name: "John", age: 25 }

// Using res.send()
app.get("/send", (req, res) => {
  res.send(user)
  // Works, but may have issues with formatting
})

// Using res.json()
app.get("/json", (req, res) => {
  res.json(user)
  // Better for APIs, proper JSON formatting
})

// HTML content
app.get("/html", (req, res) => {
  res.send("<h1>Hello</h1>") // Use send for HTML
  // res.json('<h1>Hello</h1>'); // Don't use json for HTML!
})

// API response
app.get("/api/users", (req, res) => {
  res.json({ users: [] }) // Use json for APIs
  // res.send({ users: [] }); // Works but json is clearer
})
```

**Behavior Differences:**

```javascript
// res.send() with null
app.get("/test1", (req, res) => {
  res.send(null)
  // Error! Can't send null
})

// res.json() with null
app.get("/test2", (req, res) => {
  res.json(null)
  // Works! Sends: null
})

// res.send() with boolean
app.get("/test3", (req, res) => {
  res.send(true)
  // Error! Can't send boolean
})

// res.json() with boolean
app.get("/test4", (req, res) => {
  res.json(true)
  // Works! Sends: true
})
```

**Best Practices:**

```javascript
// ✓ Use res.json() for APIs
app.get("/api/users", (req, res) => {
  res.json({ users: [] })
})

// ✓ Use res.send() for HTML
app.get("/page", (req, res) => {
  res.send("<h1>Page</h1>")
})

// ✓ Use res.json() for data responses
app.post("/users", (req, res) => {
  res.status(201).json({
    message: "Created",
    user: newUser,
  })
})
```

**Remember:**

- **res.send()** = Flexible, detects content type
- **res.json()** = Always JSON, for APIs
- For APIs, **always use res.json()**!

---

### 10. Differentiate res.redirect() vs res.send() (6 marks)

**Answer:**

| Feature            | res.send()      | res.redirect()      |
| ------------------ | --------------- | ------------------- |
| **Purpose**        | Send response   | Navigate to new URL |
| **Status Code**    | 200 (default)   | 302 (default)       |
| **Browser Action** | Display content | Go to new page      |
| **URL Change**     | No              | Yes                 |
| **Use Case**       | API responses   | After form submit   |

**res.send() - Send Content:**

```javascript
app.get("/hello", (req, res) => {
  res.send("Hello World")
  // Browser shows: "Hello World"
  // URL stays: /hello
})

app.get("/data", (req, res) => {
  res.send({ message: "Here is data" })
  // Browser shows: {"message":"Here is data"}
  // URL stays: /data
})
```

**res.redirect() - Navigate to URL:**

```javascript
app.get("/old-page", (req, res) => {
  res.redirect("/new-page")
  // Browser goes to: /new-page
  // URL changes to: /new-page
})

app.get("/new-page", (req, res) => {
  res.send("This is the new page")
})
```

**Complete Comparison Example:**

```javascript
const express = require("express")
const app = express()

app.use(express.urlencoded({ extended: true }))

let loggedIn = false

// Using res.send()
app.get("/status", (req, res) => {
  res.send(`Login status: ${loggedIn}`)
  // Shows text in browser
  // URL: /status
})

// Using res.redirect()
app.post("/login", (req, res) => {
  loggedIn = true
  res.redirect("/dashboard")
  // Browser navigates to /dashboard
  // URL changes to: /dashboard
})

app.get("/dashboard", (req, res) => {
  if (loggedIn) {
    res.send("<h1>Dashboard</h1><p>Welcome!</p>")
  } else {
    res.redirect("/login")
  }
})

app.get("/login", (req, res) => {
  res.send(`
    <form method="POST" action="/login">
      <button>Login</button>
    </form>
  `)
})
```

**Real-World Example:**

**Scenario: User Registration**

```javascript
app.post("/register", (req, res) => {
  const { username, password } = req.body

  // Save user to database
  const user = saveUser(username, password)

  // WRONG - res.send()
  res.send("User registered! Go to /login")
  // User sees message but stays on /register
  // Must manually click link

  // RIGHT - res.redirect()
  res.redirect("/login")
  // Browser automatically goes to /login
  // Better user experience!
})
```

**When to Use Each:**

**Use res.send():**

```javascript
// API endpoints
app.get("/api/users", (req, res) => {
  res.send({ users: [] })
})

// Displaying content
app.get("/about", (req, res) => {
  res.send("<h1>About Us</h1>")
})

// Direct responses
app.get("/status", (req, res) => {
  res.send("Server is running")
})
```

**Use res.redirect():**

```javascript
// After form submission
app.post("/create-post", (req, res) => {
  // Save post
  res.redirect("/posts")
})

// After login
app.post("/login", (req, res) => {
  res.redirect("/dashboard")
})

// Old URLs
app.get("/old-url", (req, res) => {
  res.redirect(301, "/new-url")
})

// Conditional redirects
app.get("/admin", (req, res) => {
  if (!isLoggedIn) {
    res.redirect("/login")
  } else {
    res.send("Admin panel")
  }
})
```

**Visual Flow:**

**res.send():**

```
Request: GET /hello
  ↓
Server: res.send('Hello')
  ↓
Browser: Shows "Hello"
URL: /hello (unchanged)
```

**res.redirect():**

```
Request: GET /old
  ↓
Server: res.redirect('/new')
  ↓
Browser: GET /new (new request)
  ↓
Server: res.send('New page')
  ↓
Browser: Shows "New page"
URL: /new (changed!)
```

**Remember:**

- **res.send()** = Show content here
- **res.redirect()** = Go to different URL
- **res.send()** = Status 200
- **res.redirect()** = Status 302

---

This completes the comprehensive Q&A guide for Unit I! All questions covered with simple, practical examples perfect for beginners.
