# React Fundamentals Q&A Guide (Simplified - Functional Components)

## Unit 1: React Web Basics

### 1. What is React? How does it differ from AngularJS?

**Answer:**

**React** is a **JavaScript library** for building user interfaces (UIs), especially for single-page applications.

**Key Features:**
- Component-based architecture
- Virtual DOM (fast updates)
- One-way data flow
- Created by Facebook (Meta)

**React vs AngularJS:**

| Feature | React | AngularJS |
|---------|-------|-----------|
| Type | Library (just UI) | Framework (complete solution) |
| Language | JavaScript/JSX | JavaScript + TypeScript |
| DOM | Virtual DOM (faster) | Real DOM |
| Data Flow | One-way | Two-way binding |
| Learning Curve | Easier | Steeper |
| File Size | Smaller | Larger |

**Simple Explanation:**
- **React** = A tool that helps you build UI (like Lego blocks)
- **AngularJS** = A complete kit with everything (like a complete toolbox)

**Example:**

```javascript
// React Component (Simple!)
function Welcome() {
  return <h1>Hello, World!</h1>;
}

// AngularJS (More complex)
angular.module('myApp', [])
  .controller('WelcomeController', function($scope) {
    $scope.message = 'Hello, World!';
  });
```

**Why React is Popular:**
- ✅ Easy to learn
- ✅ Reusable components
- ✅ Fast performance
- ✅ Large community
- ✅ Works with other libraries

---

### 2. Explain the role of NVM (Node Version Manager)

**Answer:**

**NVM** = **Node Version Manager** - A tool to manage multiple Node.js versions on your computer.

**Why Need NVM?**
Different projects need different Node.js versions:
- Project A needs Node v14
- Project B needs Node v18
- Your system has only one Node version installed

**Problem Without NVM:**
```
You: "I need Node v14 for this old project"
Computer: "I only have v18!"
You: "Ugh, I need to uninstall and reinstall..."
```

**Solution With NVM:**
```bash
# Install different Node versions
nvm install 14
nvm install 18
nvm install 20

# Switch between versions easily
nvm use 14   # Now using Node v14
nvm use 18   # Now using Node v18
```

**Common NVM Commands:**

```bash
# See installed versions
nvm list

# Install a version
nvm install 18

# Use a specific version
nvm use 18

# Set default version
nvm alias default 18

# Check current version
node --version
```

**Real-World Example:**
```bash
# Working on old project
cd old-project
nvm use 14
npm install
npm start

# Switch to new project
cd new-project
nvm use 18
npm install
npm start
```

**Benefits:**
- ✅ Easy version switching
- ✅ No conflicts between projects
- ✅ Test code on different Node versions
- ✅ No need to uninstall/reinstall

**Remember:** NVM = Node Version Manager (switch Node versions easily)

---

### 3. What is JSX? Write a short example

**Answer:**

**JSX** = **JavaScript + XML** - A syntax that looks like HTML but works in JavaScript.

**Simple Definition:** JSX lets you write HTML-like code inside JavaScript.

**Why JSX?**
- Easier to read than regular JavaScript
- Looks like HTML (familiar)
- More powerful than HTML

**Without JSX (Hard to Read):**
```javascript
// Regular JavaScript - messy!
const element = React.createElement(
  'div',
  { className: 'container' },
  React.createElement('h1', null, 'Hello'),
  React.createElement('p', null, 'Welcome to React')
);
```

**With JSX (Easy to Read!):**
```javascript
// JSX - clean and simple!
const element = (
  <div className="container">
    <h1>Hello</h1>
    <p>Welcome to React</p>
  </div>
);
```

**JSX Embedded in HTML Example:**

```javascript
// App.js
function App() {
  const name = "John";
  const age = 25;
  
  return (
    <div>
      {/* JSX looks like HTML */}
      <h1>Student Profile</h1>
      
      {/* Can use JavaScript inside {} */}
      <p>Name: {name}</p>
      <p>Age: {age}</p>
      <p>Birth Year: {2024 - age}</p>
      
      {/* Conditional rendering */}
      {age >= 18 ? <p>Adult</p> : <p>Minor</p>}
      
      {/* className instead of class */}
      <button className="btn-primary">Click Me</button>
    </div>
  );
}
```

**JSX Rules:**

1. **Use `className` not `class`:**
```javascript
<div className="container">  {/* ✓ Correct */}
<div class="container">      {/* ✗ Wrong */}
```

2. **Close all tags:**
```javascript
<img src="photo.jpg" />  {/* ✓ Correct - self-closing */}
<img src="photo.jpg">    {/* ✗ Wrong */}
```

3. **Use `{}` for JavaScript:**
```javascript
<h1>2 + 2 = {2 + 2}</h1>  {/* Shows: 2 + 2 = 4 */}
```

4. **Return single parent:**
```javascript
// ✓ Correct - wrapped in div
return (
  <div>
    <h1>Title</h1>
    <p>Text</p>
  </div>
);

// ✗ Wrong - two parents
return (
  <h1>Title</h1>
  <p>Text</p>
);
```

**Remember:** JSX = HTML-like syntax in JavaScript

---

### 4. List five advantages of component composition in React

**Answer:**

**Component Composition** = Building complex UIs by combining smaller components (like Lego blocks).

**5 Key Advantages:**

**1. Reusability** - Use same component multiple times
```javascript
// Button component (write once, use everywhere)
function Button({ text, onClick }) {
  return <button onClick={onClick}>{text}</button>;
}

// Use it many times
function App() {
  return (
    <div>
      <Button text="Save" onClick={handleSave} />
      <Button text="Cancel" onClick={handleCancel} />
      <Button text="Delete" onClick={handleDelete} />
    </div>
  );
}
```

**2. Maintainability** - Easy to fix/update
```javascript
// If Button needs changes, update once, affects everywhere
function Button({ text, onClick }) {
  return (
    <button 
      className="modern-btn"  {/* Added style */}
      onClick={onClick}
    >
      {text}
    </button>
  );
}
// All buttons now have new style!
```

**3. Readability** - Code is cleaner
```javascript
// Without composition (messy)
function App() {
  return (
    <div>
      <div className="card">
        <h2>Student 1</h2>
        <p>Age: 20</p>
      </div>
      <div className="card">
        <h2>Student 2</h2>
        <p>Age: 22</p>
      </div>
    </div>
  );
}

// With composition (clean!)
function StudentCard({ name, age }) {
  return (
    <div className="card">
      <h2>{name}</h2>
      <p>Age: {age}</p>
    </div>
  );
}

function App() {
  return (
    <div>
      <StudentCard name="Student 1" age={20} />
      <StudentCard name="Student 2" age={22} />
    </div>
  );
}
```

**4. Testability** - Easy to test small parts
```javascript
// Test Button component separately
test('Button shows correct text', () => {
  render(<Button text="Click Me" />);
  expect(screen.getByText('Click Me')).toBeInTheDocument();
});
```

**5. Team Collaboration** - Different people work on different components
```
Developer A: Works on Header component
Developer B: Works on Footer component
Developer C: Works on Content component
Developer D: Combines all in App component
```

**Real Example - Building a Profile:**
```javascript
// Small components
function Avatar({ src }) {
  return <img src={src} alt="avatar" />;
}

function UserName({ name }) {
  return <h2>{name}</h2>;
}

function Bio({ text }) {
  return <p>{text}</p>;
}

// Composed together
function UserProfile({ user }) {
  return (
    <div className="profile">
      <Avatar src={user.avatar} />
      <UserName name={user.name} />
      <Bio text={user.bio} />
    </div>
  );
}
```

**Remember:** Small components = Easy to manage!

---

### 5. What is the purpose of `npm init` in a React project?

**Answer:**

**`npm init`** creates a **`package.json`** file - the "instruction manual" for your project.

**What is `package.json`?**
A file that stores:
- Project name and version
- Dependencies (libraries you need)
- Scripts (commands to run)
- Author information

**How to Use:**
```bash
# Interactive mode (asks questions)
npm init

# Quick mode (uses defaults)
npm init -y
```

**What it asks (interactive mode):**
```
package name: (my-react-app) 
version: (1.0.0) 
description: My first React app
entry point: (index.js) 
test command: 
git repository: 
keywords: 
author: Your Name
license: (ISC) 
```

**Resulting `package.json`:**
```json
{
  "name": "my-react-app",
  "version": "1.0.0",
  "description": "My first React app",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Your Name",
  "license": "ISC"
}
```

**Why is it Important?**

**1. Tracks Dependencies:**
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  }
}
```

**2. Shares Project Easily:**
```bash
# You share project (without node_modules)
# Others run:
npm install  # Installs all dependencies from package.json
```

**3. Defines Scripts:**
```json
{
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test"
  }
}
```

**4. Version Management:**
```json
{
  "version": "1.0.0"  // Track your app version
}
```

**Real-World Use:**
```bash
# Create new React project
mkdir my-app
cd my-app
npm init -y              # Creates package.json
npm install react react-dom  # Adds dependencies
```

**Remember:** `npm init` = Creates package.json (project config file)

---

### 6. Describe the role of `this.props.children` (Modern: `props.children`)

**Answer:**

**`props.children`** contains whatever you put **between** component tags.

**Think of it like a wrapper/container:**
```
<Box>
  [This content goes into props.children]
</Box>
```

**Simple Example:**

```javascript
// Container component
function Box({ children }) {
  return (
    <div className="box">
      {children}  {/* Content appears here */}
    </div>
  );
}

// Using the container
function App() {
  return (
    <Box>
      <h1>Hello!</h1>
      <p>This is inside the box</p>
    </Box>
  );
}

// Renders:
// <div class="box">
//   <h1>Hello!</h1>
//   <p>This is inside the box</p>
// </div>
```

**Real-World Examples:**

**1. Card Component:**
```javascript
function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

// Use it
<Card>
  <h2>Student Name</h2>
  <p>Age: 20</p>
  <button>View Details</button>
</Card>
```

**2. Modal/Popup:**
```javascript
function Modal({ children }) {
  return (
    <div className="modal-overlay">
      <div className="modal-content">
        {children}
      </div>
    </div>
  );
}

// Use it
<Modal>
  <h2>Confirm Delete</h2>
  <p>Are you sure?</p>
  <button>Yes</button>
  <button>No</button>
</Modal>
```

**3. Layout Component:**
```javascript
function Layout({ children }) {
  return (
    <div>
      <header>My App</header>
      <main>
        {children}  {/* Page content */}
      </main>
      <footer>© 2024</footer>
    </div>
  );
}

// Use it
<Layout>
  <h1>Welcome</h1>
  <p>This is the home page</p>
</Layout>
```

**Multiple Children:**
```javascript
function App() {
  return (
    <Box>
      <h1>Title</h1>
      <p>Paragraph</p>
      <button>Click</button>
      {/* All of these are in props.children as array */}
    </Box>
  );
}
```

**Why Use `children`?**
- ✅ Flexible wrapper components
- ✅ Reusable layouts
- ✅ Clean, readable code
- ✅ Component composition

**Remember:** `children` = Content between component tags

---

### 7. What is property validation in React? Give an example

**Answer:**

**Property Validation** (PropTypes) = Checking if props have the **correct data type**.

**Why Validate?**
- Catch errors early
- Better code documentation
- Team knows what props to pass

**Setup:**
```bash
npm install prop-types
```

**Simple Example:**

```javascript
import PropTypes from 'prop-types';

// Component
function StudentCard({ name, age, email }) {
  return (
    <div className="card">
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
    </div>
  );
}

// Property Validation
StudentCard.propTypes = {
  name: PropTypes.string.isRequired,   // Must be string, required
  age: PropTypes.number.isRequired,    // Must be number, required
  email: PropTypes.string              // Must be string, optional
};

// Default values (if prop not provided)
StudentCard.defaultProps = {
  email: 'No email provided'
};
```

**Using the Component:**

```javascript
// ✓ Correct - all props valid
<StudentCard name="John" age={20} email="john@example.com" />

// ✓ Correct - email optional
<StudentCard name="John" age={20} />

// ✗ Wrong - age is string (should be number)
<StudentCard name="John" age="20" />
// Console Warning: Invalid prop `age` of type `string`

// ✗ Wrong - missing required prop
<StudentCard name="John" />
// Console Warning: Required prop `age` was not specified
```

**Common PropTypes:**

```javascript
import PropTypes from 'prop-types';

Component.propTypes = {
  // Basic types
  name: PropTypes.string,
  age: PropTypes.number,
  isActive: PropTypes.bool,
  onClick: PropTypes.func,
  items: PropTypes.array,
  user: PropTypes.object,
  
  // Required
  id: PropTypes.number.isRequired,
  
  // Specific values
  status: PropTypes.oneOf(['pending', 'approved', 'rejected']),
  
  // Array of specific type
  scores: PropTypes.arrayOf(PropTypes.number),
  
  // Object with specific shape
  user: PropTypes.shape({
    name: PropTypes.string,
    age: PropTypes.number
  }),
  
  // Any type (not recommended)
  data: PropTypes.any
};
```

**Real Example - Product Component:**

```javascript
import PropTypes from 'prop-types';

function Product({ id, name, price, inStock, onBuy }) {
  return (
    <div className="product">
      <h3>{name}</h3>
      <p>Price: ${price}</p>
      <p>{inStock ? 'In Stock' : 'Out of Stock'}</p>
      <button onClick={onBuy} disabled={!inStock}>
        Buy Now
      </button>
    </div>
  );
}

Product.propTypes = {
  id: PropTypes.number.isRequired,
  name: PropTypes.string.isRequired,
  price: PropTypes.number.isRequired,
  inStock: PropTypes.bool,
  onBuy: PropTypes.func
};

Product.defaultProps = {
  inStock: true,
  onBuy: () => console.log('No handler provided')
};
```

**Benefits:**
- ✅ Catches bugs early
- ✅ Self-documenting code
- ✅ Better IDE autocomplete
- ✅ Easier for team members

**Remember:** PropTypes = Type checking for props (like a safety net)

---

### 8. Write a React component that displays a list of continents using map()

**Answer:**

```javascript
function ContinentList() {
  // Array of continents
  const continents = [
    'Asia',
    'Africa',
    'North America',
    'South America',
    'Europe',
    'Australia',
    'Antarctica'
  ];
  
  return (
    <div>
      <h1>Continents of the World</h1>
      <ul>
        {/* Use map() to create list items */}
        {continents.map((continent, index) => (
          <li key={index}>{continent}</li>
        ))}
      </ul>
    </div>
  );
}

export default ContinentList;
```

**How `map()` Works:**
```javascript
// map() transforms array into JSX elements
continents.map((continent) => <li>{continent}</li>)

// Step by step:
'Asia' → <li>Asia</li>
'Africa' → <li>Africa</li>
'Europe' → <li>Europe</li>
```

**With More Details:**

```javascript
function ContinentList() {
  const continents = [
    { id: 1, name: 'Asia', countries: 48 },
    { id: 2, name: 'Africa', countries: 54 },
    { id: 3, name: 'North America', countries: 23 },
    { id: 4, name: 'South America', countries: 12 },
    { id: 5, name: 'Europe', countries: 44 },
    { id: 6, name: 'Australia', countries: 14 },
    { id: 7, name: 'Antarctica', countries: 0 }
  ];
  
  return (
    <div>
      <h1>Continents</h1>
      <div className="continent-grid">
        {continents.map((continent) => (
          <div key={continent.id} className="continent-card">
            <h3>{continent.name}</h3>
            <p>Countries: {continent.countries}</p>
          </div>
        ))}
      </div>
    </div>
  );
}
```

**Why Use `key`?**
```javascript
{continents.map((continent) => (
  <li key={continent.id}>  {/* ← IMPORTANT! */}
    {continent.name}
  </li>
))}
```

**Key** helps React identify which items changed:
- ✅ Better performance
- ✅ Prevents bugs
- ✅ Should be unique (use id, not index if possible)

**Complete Example with Styling:**

```javascript
import './ContinentList.css';

function ContinentList() {
  const continents = [
    { id: 1, name: 'Asia', area: '44.58 million km²', population: '4.6 billion' },
    { id: 2, name: 'Africa', area: '30.37 million km²', population: '1.3 billion' },
    { id: 3, name: 'North America', area: '24.71 million km²', population: '579 million' },
    { id: 4, name: 'South America', area: '17.84 million km²', population: '430 million' },
    { id: 5, name: 'Europe', area: '10.18 million km²', population: '746 million' },
    { id: 6, name: 'Australia', area: '8.6 million km²', population: '42 million' }
  ];
  
  return (
    <div className="container">
      <h1>World Continents</h1>
      <div className="grid">
        {continents.map((continent) => (
          <div key={continent.id} className="card">
            <h2>{continent.name}</h2>
            <p><strong>Area:</strong> {continent.area}</p>
            <p><strong>Population:</strong> {continent.population}</p>
          </div>
        ))}
      </div>
    </div>
  );
}

export default ContinentList;
```

**Remember:** `map()` = Loop through array and create JSX elements

---

## MAJOR QUESTION (10 MARKS)

### Student Details App with Functional Components

**Answer:**

```javascript
import React from 'react';
import './App.css';

// 1. EMPLOYEE COMPONENT (Personal Details)
function Employee({ employee }) {
  return (
    <div className="employee-section">
      <h2>Employee Details</h2>
      <div className="details">
        <p><strong>ID:</strong> {employee.id}</p>
        <p><strong>Name:</strong> {employee.name}</p>
        <p><strong>Email:</strong> {employee.email}</p>
        <p><strong>Phone:</strong> {employee.phone}</p>
        <p><strong>Department:</strong> {employee.department}</p>
      </div>
    </div>
  );
}

// 2. SKILLS COMPONENT
function Skills({ skills }) {
  return (
    <div className="skills-section">
      <h2>Skills</h2>
      <div className="skills-grid">
        {skills.map((skill, index) => (
          <div key={index} className="skill-card">
            <h3>{skill.name}</h3>
            <div className="skill-bar">
              <div 
                className="skill-level" 
                style={{ width: `${skill.level}%` }}
              >
                {skill.level}%
              </div>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
}

// 3. PARENT COMPONENT (Combines everything)
function StudentProfile() {
  // Sample data
  const employeeData = {
    id: 'EMP001',
    name: 'John Doe',
    email: 'john.doe@example.com',
    phone: '+1-555-0123',
    department: 'Computer Science'
  };
  
  const skillsData = [
    { name: 'JavaScript', level: 90 },
    { name: 'React', level: 85 },
    { name: 'Node.js', level: 80 },
    { name: 'Python', level: 75 },
    { name: 'SQL', level: 70 }
  ];
  
  return (
    <div className="profile-container">
      <h1>Student Profile</h1>
      
      {/* Render Employee component */}
      <Employee employee={employeeData} />
      
      {/* Render Skills component */}
      <Skills skills={skillsData} />
    </div>
  );
}

export default StudentProfile;
```

**CSS (App.css):**
```css
.profile-container {
  max-width: 800px;
  margin: 20px auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.profile-container h1 {
  text-align: center;
  color: #333;
  border-bottom: 3px solid #007bff;
  padding-bottom: 10px;
}

.employee-section, .skills-section {
  background: #f8f9fa;
  padding: 20px;
  margin: 20px 0;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.employee-section h2, .skills-section h2 {
  color: #007bff;
  margin-bottom: 15px;
}

.details p {
  margin: 10px 0;
  font-size: 16px;
}

.skills-grid {
  display: grid;
  gap: 15px;
}

.skill-card h3 {
  margin-bottom: 10px;
  color: #333;
}

.skill-bar {
  background: #e9ecef;
  border-radius: 10px;
  overflow: hidden;
  height: 30px;
}

.skill-level {
  background: linear-gradient(90deg, #007bff, #0056b3);
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: bold;
  transition: width 0.3s ease;
}
```

**Explanation:**

**Component Structure:**
```
StudentProfile (Parent)
  ├── Employee (Child 1)
  └── Skills (Child 2)
```

**How Props Work:**
```javascript
// Parent passes data to children
<Employee employee={employeeData} />
           ↓
// Child receives via props
function Employee({ employee }) {
  // Use employee.name, employee.email, etc.
}
```

**Output:**
```
┌─────────────────────────────────┐
│     Student Profile             │
├─────────────────────────────────┤
│  Employee Details               │
│  ID: EMP001                     │
│  Name: John Doe                 │
│  Email: john.doe@example.com    │
│  Phone: +1-555-0123             │
│  Department: Computer Science   │
├─────────────────────────────────┤
│  Skills                         │
│  ┌─────────────────────┐        │
│  │ JavaScript  [90%]   │        │
│  │ React      [85%]    │        │
│  │ Node.js    [80%]    │        │
│  │ Python     [75%]    │        │
│  │ SQL        [70%]    │        │
│  └─────────────────────┘        │
└─────────────────────────────────┘
```

**Key Concepts Used:**
- ✅ Functional components
- ✅ Props for passing data
- ✅ Component composition
- ✅ map() for rendering lists
- ✅ Destructuring props

---

### Create IssueTable Component

**Answer:**

```javascript
import React from 'react';
import './IssueTable.css';

// IssueTable Component
function IssueTable({ issues }) {
  return (
    <div className="table-container">
      <h1>Issue Tracker</h1>
      
      <table className="issue-table">
        <thead>
          <tr>
            <th>ID</th>
            <th>Title</th>
            <th>Status</th>
            <th>Priority</th>
          </tr>
        </thead>
        <tbody>
          {issues.map((issue) => (
            <tr key={issue.id}>
              <td>{issue.id}</td>
              <td>{issue.title}</td>
              <td>
                <span className={`status ${issue.status.toLowerCase()}`}>
                  {issue.status}
                </span>
              </td>
              <td>
                <span className={`priority ${issue.priority.toLowerCase()}`}>
                  {issue.priority}
                </span>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
      
      <p className="total">Total Issues: {issues.length}</p>
    </div>
  );
}

// App Component (Parent)
function App() {
  // Sample issues data
  const issuesData = [
    { id: 1, title: 'Login page not working', status: 'Open', priority: 'High' },
    { id: 2, title: 'Add dark mode feature', status: 'In Progress', priority: 'Medium' },
    { id: 3, title: 'Fix navbar alignment', status: 'Open', priority: 'Low' },
    { id: 4, title: 'Database connection error', status: 'Closed', priority: 'High' },
    { id: 5, title: 'Update user profile page', status: 'In Progress', priority: 'Medium' }
  ];
  
  return (
    <div className="app">
      {/* Pass issues array as prop */}
      <IssueTable issues={issuesData} />
    </div>
  );
}

export default App;
```

**CSS (IssueTable.css):**
```css
.table-container {
  max-width: 1000px;
  margin: 20px auto;
  padding: 20px;
}

.table-container h1 {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

.issue-table {
  width: 100%;
  border-collapse: collapse;
  background: white;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  border-radius: 8px;
  overflow: hidden;
}

.issue-table thead {
  background: #007bff;
  color: white;
}

.issue-table th {
  padding: 15px;
  text-align: left;
  font-weight: 600;
  font-size: 14px;
  text-transform: uppercase;
}

.issue-table tbody tr {
  border-bottom: 1px solid #e9ecef;
  transition: background 0.2s;
}

.issue-table tbody tr:hover {
  background: #f8f9fa;
}

.issue-table td {
  padding: 15px;
  font-size: 14px;
}

.status, .priority {
  padding: 5px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 600;
}

.status.open {
  background: #ffc107;
  color: #000;
}

.status.closed {
  background: #28a745;
  color: white;
}

.status.in.progress {
  background: #17a2b8;
  color: white;
}

.priority.high {
  background: #dc3545;
  color: white;
}

.priority.medium {
  background: #fd7e14;
  color: white;
}

.priority.low {
  background: #6c757d;
  color: white;
}

.total {
  text-align: right;
  margin-top: 15px;
  font-weight: 600;
  color: #666;
}
```

**How it Works:**

**1. Parent passes data:**
```javascript
<IssueTable issues={issuesData} />
```

**2. Child receives and maps:**
```javascript
function IssueTable({ issues }) {
  return (
    <table>
      <tbody>
        {issues.map((issue) => (
          <tr key={issue.id}>
            <td>{issue.id}</td>
            <td>{issue.title}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

**3. Each issue becomes a row:**
```
Issue: { id: 1, title: 'Login bug' }
  ↓
Row: <tr><td>1</td><td>Login bug</td></tr>
```

**Output:**
```
┌─────────────────────────────────────────────────────┐
│              Issue Tracker                          │
├────┬──────────────────────┬────────────┬──────────┤
│ ID │ Title                │ Status     │ Priority │
├────┼──────────────────────┼────────────┼──────────┤
│ 1  │ Login page not work  │ Open       │ High     │
│ 2  │ Add dark mode        │ In Progress│ Medium   │
│ 3  │ Fix navbar           │ Open       │ Low      │
│ 4  │ Database error       │ Closed     │ High     │
│ 5  │ Update profile       │ In Progress│ Medium   │
└────┴──────────────────────┴────────────┴──────────┘
Total Issues: 5
```

**Key Concepts:**
- ✅ Props (issues array)
- ✅ map() to create rows
- ✅ key prop (unique id)
- ✅ Dynamic styling
- ✅ Conditional classes

---

## Unit 2: React State

### 1. Difference between State and Props

**Answer:**

**State** = Data **inside** a component (component owns it)
**Props** = Data **passed from parent** (component receives it)

**Comparison Table:**

| Feature | State | Props |
|---------|-------|-------|
| **Ownership** | Owned by component | Passed from parent |
| **Change** | Can be changed | Read-only |
| **Who updates** | Component itself | Parent component |
| **Use** | Internal data | Communication |
| **Example** | Counter value | User name |

**Simple Example:**

```javascript
// PROPS Example (passed from parent)
function Welcome({ name }) {  // name is a prop
  return <h1>Hello, {name}!</h1>;
}

// Parent passes prop
<Welcome name="John" />  // John is passed down
```

```javascript
// STATE Example (internal to component)
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);  // count is state
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

**Real-World Analogy:**

**Props = Gifts from parents:**
- Your parents give you a gift
- You can use it but can't change it
- They decide what to give

**State = Your own belongings:**
- You own your toys
- You can change/organize them
- You control them

**Combined Example:**

```javascript
import { useState } from 'react';

// Child component (uses props)
function Counter({ initialValue, title }) {
  // State (internal)
  const [count, setCount] = useState(initialValue);
  
  return (
    <div>
      <h2>{title}</h2>  {/* Prop - from parent */}
      <p>Count: {count}</p>  {/* State - internal */}
      <button onClick={() => setCount(count + 1)}>
        +
      </button>
    </div>
  );
}

// Parent component
function App() {
  return (
    <div>
      {/* Pass props to children */}
      <Counter initialValue={0} title="Counter A" />
      <Counter initialValue={10} title="Counter B" />
    </div>
  );
}
```

**Key Differences:**

**Props:**
```javascript
function Greeting({ name }) {
  // name = "John" (received from parent)
  // name = "Jane"  ← ERROR! Can't change props
  return <h1>Hello, {name}</h1>;
}
```

**State:**
```javascript
function Greeting() {
  const [name, setName] = useState("John");
  
  // Can change state
  setName("Jane");  // ✓ Allowed!
  
  return <h1>Hello, {name}</h1>;
}
```

**Remember:**
- **Props** = Passed down (read-only)
- **State** = Internal (can change)

---

### 2. Explain Stateless Component with Example

**Answer:**

**Stateless Component** = Component **without state** (only receives props and renders UI).

**Characteristics:**
- No useState() hook
- Only displays data
- Receives everything via props
- Also called "Presentational Component"

**Simple Example:**

```javascript
// Stateless Component
function UserCard({ name, email, age }) {
  return (
    <div className="card">
      <h2>{name}</h2>
      <p>Email: {email}</p>
      <p>Age: {age}</p>
    </div>
  );
}

// Usage
<UserCard 
  name="John Doe" 
  email="john@example.com" 
  age={25} 
/>
```

**More Examples:**

**1. Button Component:**
```javascript
// Stateless - just displays a button
function Button({ text, onClick, color }) {
  return (
    <button 
      onClick={onClick}
      style={{ backgroundColor: color }}
    >
      {text}
    </button>
  );
}

// Usage
<Button 
  text="Click Me" 
  onClick={handleClick} 
  color="blue" 
/>
```

**2. Product Card:**
```javascript
// Stateless - just displays product info
function ProductCard({ product }) {
  return (
    <div className="product">
      <img src={product.image} alt={product.name} />
      <h3>{product.name}</h3>
      <p>${product.price}</p>
      <p>{product.description}</p>
    </div>
  );
}

// Usage
<ProductCard product={productData} />
```

**3. Header Component:**
```javascript
// Stateless - just shows header
function Header({ title, subtitle }) {
  return (
    <header>
      <h1>{title}</h1>
      <p>{subtitle}</p>
    </header>
  );
}

// Usage
<Header 
  title="My Website" 
  subtitle="Welcome to our site" 
/>
```

**Contrast with Stateful Component:**

```javascript
// STATELESS (no state)
function Display({ count }) {
  return <p>Count: {count}</p>;
}

// STATEFUL (has state)
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <Display count={count} />
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

**Complete Example:**

```javascript
// Stateless Component
function StudentCard({ student }) {
  return (
    <div className="student-card">
      <div className="header">
        <img src={student.photo} alt={student.name} />
        <h2>{student.name}</h2>
      </div>
      <div className="details">
        <p><strong>Roll No:</strong> {student.rollNo}</p>
        <p><strong>Branch:</strong> {student.branch}</p>
        <p><strong>Year:</strong> {student.year}</p>
        <p><strong>GPA:</strong> {student.gpa}</p>
      </div>
      <div className="subjects">
        <h3>Subjects:</h3>
        <ul>
          {student.subjects.map((subject, index) => (
            <li key={index}>{subject}</li>
          ))}
        </ul>
      </div>
    </div>
  );
}

// Parent (Stateful) Component
function StudentList() {
  const students = [
    {
      id: 1,
      name: "Alice",
      photo: "alice.jpg",
      rollNo: "CS001",
      branch: "Computer Science",
      year: "3rd",
      gpa: 3.8,
      subjects: ["Data Structures", "Algorithms", "Web Dev"]
    },
    {
      id: 2,
      name: "Bob",
      photo: "bob.jpg",
      rollNo: "CS002",
      branch: "Computer Science",
      year: "3rd",
      gpa: 3.6,
      subjects: ["OS", "Networks", "Database"]
    }
  ];
  
  return (
    <div className="student-list">
      <h1>Students</h1>
      {students.map(student => (
        <StudentCard key={student.id} student={student} />
      ))}
    </div>
  );
}
```

**Benefits of Stateless Components:**
- ✅ Simple and easy to understand
- ✅ Easy to test
- ✅ Reusable
- ✅ Better performance
- ✅ Pure functions (same input = same output)

**When to Use:**
- Display components (cards, buttons, headers)
- UI elements that just show data
- Components that don't need to track changes

**Remember:** Stateless = No state, only props (simple display)

---

### 3. Explain the purpose of `setState()` (Modern: `useState`)

**Answer:**

In modern React (functional components), we use **`useState()`** hook instead of `this.setState()`.

**Purpose:** To **update component data** and **trigger re-render**.

**How useState Works:**

```javascript
import { useState } from 'react';

function Counter() {
  // Declare state
  const [count, setCount] = useState(0);
  //      ↑        ↑            ↑
  //   value   updater    initial value
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

**Breaking it down:**

```javascript
const [count, setCount] = useState(0);
```
- `count` = current state value
- `setCount` = function to update count
- `useState(0)` = initial value is 0

**Why We Need It:**

**❌ Wrong Way (doesn't work):**
```javascript
function Counter() {
  let count = 0;  // Regular variable
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => count = count + 1}>
        Increment  {/* Doesn't re-render! */}
      </button>
    </div>
  );
}
```

**✓ Correct Way (with useState):**
```javascript
function Counter() {
  const [count, setCount] = useState(0);  // State
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment  {/* Re-renders! */}
      </button>
    </div>
  );
}
```

**Multiple State Variables:**

```javascript
function UserForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [age, setAge] = useState(0);
  
  return (
    <form>
      <input 
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Name"
      />
      
      <input 
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      
      <input 
        type="number"
        value={age}
        onChange={(e) => setAge(Number(e.target.value))}
        placeholder="Age"
      />
    </form>
  );
}
```

**State with Objects:**

```javascript
function UserProfile() {
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: 0
  });
  
  // Update single property
  const updateName = (newName) => {
    setUser({
      ...user,  // Keep other properties
      name: newName  // Update only name
    });
  };
  
  return (
    <div>
      <input 
        value={user.name}
        onChange={(e) => updateName(e.target.value)}
      />
    </div>
  );
}
```

**State with Arrays:**

```javascript
function TodoList() {
  const [todos, setTodos] = useState([]);
  
  // Add item
  const addTodo = (text) => {
    setTodos([...todos, { id: Date.now(), text }]);
  };
  
  // Remove item
  const removeTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };
  
  return (
    <div>
      {todos.map(todo => (
        <div key={todo.id}>
          {todo.text}
          <button onClick={() => removeTodo(todo.id)}>
            Delete
          </button>
        </div>
      ))}
    </div>
  );
}
```

**Important Rules:**

**1. Never Modify State Directly:**
```javascript
// ❌ Wrong
count = count + 1;
user.name = "John";

// ✓ Correct
setCount(count + 1);
setUser({ ...user, name: "John" });
```

**2. State Updates are Asynchronous:**
```javascript
setCount(5);
console.log(count);  // Still shows old value!
```

**3. Use Functional Updates for Current Value:**
```javascript
// ✓ Best practice
setCount(prevCount => prevCount + 1);
```

**Remember:** useState = Declare state + get updater function

---

### 4. Why should props not be copied into state?

**Answer:**

**Rule:** Don't copy props to state unless you have a very good reason!

**Why It's Bad:**

**Problem 1: Out of Sync**
```javascript
// ❌ Bad: Copying prop to state
function Counter({ initialCount }) {
  const [count, setCount] = useState(initialCount);
  
  // If parent changes initialCount, this won't update!
  return <p>Count: {count}</p>;
}
```

**What happens:**
```
Parent passes initialCount = 5
  ↓
Child sets state: count = 5
  ↓
Parent changes initialCount = 10
  ↓
Child still shows count = 5  ← Problem!
```

**Solution: Just use the prop directly**
```javascript
// ✓ Good: Use prop directly
function Counter({ count }) {
  return <p>Count: {count}</p>;
}
```

**Problem 2: Confusing Source of Truth**
```javascript
// ❌ Confusing: Which value is correct?
function UserCard({ userName }) {
  const [name, setName] = useState(userName);
  
  // Is the correct value:
  // - userName (prop)?
  // - name (state)?
  // Hard to tell!
}
```

**When It's OK to Copy:**

**Only when you need a "default value" that can be changed:**

```javascript
// ✓ Acceptable: Initial value that user can change
function ColorPicker({ defaultColor }) {
  const [color, setColor] = useState(defaultColor);
  
  return (
    <div>
      <input 
        type="color"
        value={color}
        onChange={(e) => setColor(e.target.value)}
      />
      <p>Selected: {color}</p>
    </div>
  );
}

// Parent sets default, but user can change it
<ColorPicker defaultColor="#ff0000" />
```

**Better Approach - Controlled Component:**

```javascript
// ✓ Best: Parent controls the state
function App() {
  const [color, setColor] = useState('#ff0000');
  
  return (
    <ColorPicker 
      color={color}  // Prop
      onChange={setColor}  // Callback
    />
  );
}

function ColorPicker({ color, onChange }) {
  return (
    <input 
      type="color"
      value={color}
      onChange={(e) => onChange(e.target.value)}
    />
  );
}
```

**Real Example - Form Input:**

```javascript
// ❌ Bad: Copying prop to state
function NameInput({ userName }) {
  const [name, setName] = useState(userName);
  
  return (
    <input value={name} onChange={(e) => setName(e.target.value)} />
  );
}

// ✓ Good: Controlled by parent
function NameInput({ name, onChange }) {
  return (
    <input value={name} onChange={(e) => onChange(e.target.value)} />
  );
}

// Parent component
function App() {
  const [userName, setUserName] = useState('John');
  
  return (
    <NameInput name={userName} onChange={setUserName} />
  );
}
```

**Exception with useEffect:**

```javascript
// If you MUST copy, sync with useEffect
function Counter({ initialCount }) {
  const [count, setCount] = useState(initialCount);
  
  // Update state when prop changes
  useEffect(() => {
    setCount(initialCount);
  }, [initialCount]);
  
  return <p>Count: {count}</p>;
}
```

**Remember:**
- ❌ Don't copy props to state
- ✓ Use props directly
- ✓ Or lift state up to parent

---

### MAJOR QUESTION: Event Handling with State Update

**Answer:**

```javascript
import { useState } from 'react';
import './App.css';

function App() {
  // State to store button click count
  const [clickCount, setClickCount] = useState(0);
  const [message, setMessage] = useState('Click the button!');
  const [lastClicked, setLastClicked] = useState(null);
  
  // Event handler for button click
  const handleClick = () => {
    // Update count
    setClickCount(clickCount + 1);
    
    // Update message
    setMessage(`Button clicked ${clickCount + 1} times!`);
    
    // Update timestamp
    setLastClicked(new Date().toLocaleTimeString());
  };
  
  // Event handler for reset
  const handleReset = () => {
    setClickCount(0);
    setMessage('Click the button!');
    setLastClicked(null);
  };
  
  return (
    <div className="app">
      <h1>Event Handling Demo</h1>
      
      <div className="display">
        <h2>{message}</h2>
        <p className="count">Total Clicks: {clickCount}</p>
        
        {lastClicked && (
          <p className="timestamp">
            Last clicked at: {lastClicked}
          </p>
        )}
      </div>
      
      <div className="buttons">
        <button 
          className="click-btn"
          onClick={handleClick}
        >
          Click Me!
        </button>
        
        <button 
          className="reset-btn"
          onClick={handleReset}
        >
          Reset
        </button>
      </div>
      
      {/* Show message based on count */}
      {clickCount >= 10 && (
        <p className="achievement">
          🎉 You're on fire! {clickCount} clicks!
        </p>
      )}
    </div>
  );
}

export default App;
```

**CSS (App.css):**
```css
.app {
  max-width: 600px;
  margin: 50px auto;
  padding: 30px;
  text-align: center;
  font-family: Arial, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 20px;
  color: white;
  box-shadow: 0 10px 30px rgba(0,0,0,0.3);
}

.app h1 {
  margin-bottom: 30px;
  font-size: 32px;
}

.display {
  background: rgba(255,255,255,0.1);
  padding: 30px;
  border-radius: 15px;
  margin-bottom: 20px;
  backdrop-filter: blur(10px);
}

.display h2 {
  font-size: 24px;
  margin-bottom: 15px;
}

.count {
  font-size: 48px;
  font-weight: bold;
  margin: 20px 0;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

.timestamp {
  font-size: 14px;
  opacity: 0.8;
}

.buttons {
  display: flex;
  gap: 15px;
  justify-content: center;
  margin-top: 20px;
}

button {
  padding: 15px 30px;
  font-size: 18px;
  font-weight: bold;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s;
}

.click-btn {
  background: #4CAF50;
  color: white;
}

.click-btn:hover {
  background: #45a049;
  transform: scale(1.05);
}

.click-btn:active {
  transform: scale(0.95);
}

.reset-btn {
  background: #f44336;
  color: white;
}

.reset-btn:hover {
  background: #da190b;
  transform: scale(1.05);
}

.achievement {
  margin-top: 20px;
  font-size: 20px;
  animation: bounce 0.5s;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}
```

**How It Works:**

**1. State Declaration:**
```javascript
const [clickCount, setClickCount] = useState(0);
```
- Initial value: 0
- Updates when button clicked

**2. Event Handler:**
```javascript
const handleClick = () => {
  setClickCount(clickCount + 1);
};
```
- Function that runs on click
- Updates state

**3. Attach to Button:**
```javascript
<button onClick={handleClick}>
  Click Me!
</button>
```
- `onClick` = event
- `handleClick` = handler function

**4. Re-render:**
```
Click button
  ↓
handleClick() runs
  ↓
setClickCount() called
  ↓
Component re-renders
  ↓
New count displayed
```

**Complete Flow:**
```
User clicks "Click Me!" button
  ↓
onClick event fires
  ↓
handleClick() function executes
  ↓
setClickCount(clickCount + 1)
  ↓
State updates: 0 → 1
  ↓
React re-renders component
  ↓
Display shows "Total Clicks: 1"
```

**Key Concepts:**
- ✅ Event handling (onClick)
- ✅ State update (useState)
- ✅ Re-rendering
- ✅ Conditional rendering
- ✅ Multiple state variables

---

### MAJOR: Issue Tracker with Fetch Data

**Answer:**

```javascript
import { useState } from 'react';
import './IssueTracker.css';

function IssueTracker() {
  // State for issues list
  const [issues, setIssues] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  
  // Sample issue data (simulating API)
  const sampleIssues = [
    {
      id: 1,
      title: 'Login page not responsive',
      description: 'Login page doesn\'t work on mobile devices',
      status: 'Open',
      priority: 'High',
      createdDate: '2024-02-01'
    },
    {
      id: 2,
      title: 'Add dark mode',
      description: 'Implement dark mode theme option',
      status: 'In Progress',
      priority: 'Medium',
      createdDate: '2024-02-03'
    },
    {
      id: 3,
      title: 'Fix navbar alignment',
      description: 'Navbar items are not properly aligned',
      status: 'Open',
      priority: 'Low',
      createdDate: '2024-02-05'
    },
    {
      id: 4,
      title: 'Database connection timeout',
      description: 'Database takes too long to connect',
      status: 'Closed',
      priority: 'High',
      createdDate: '2024-01-28'
    },
    {
      id: 5,
      title: 'Update profile page UI',
      description: 'Profile page needs modern redesign',
      status: 'In Progress',
      priority: 'Medium',
      createdDate: '2024-02-04'
    }
  ];
  
  // Event handler to fetch issues
  const handleFetchIssues = () => {
    setLoading(true);
    setError(null);
    
    // Simulate API call with setTimeout
    setTimeout(() => {
      try {
        // Update state with fetched issues
        setIssues(sampleIssues);
        setLoading(false);
      } catch (err) {
        setError('Failed to fetch issues');
        setLoading(false);
      }
    }, 1000); // 1 second delay to simulate network
  };
  
  // Clear issues
  const handleClearIssues = () => {
    setIssues([]);
  };
  
  return (
    <div className="issue-tracker">
      <header className="header">
        <h1>🎯 Issue Tracker</h1>
        <p>Manage and track your project issues</p>
      </header>
      
      <div className="controls">
        <button 
          className="fetch-btn"
          onClick={handleFetchIssues}
          disabled={loading}
        >
          {loading ? 'Loading...' : 'Fetch Issues'}
        </button>
        
        {issues.length > 0 && (
          <button 
            className="clear-btn"
            onClick={handleClearIssues}
          >
            Clear All
          </button>
        )}
      </div>
      
      {error && (
        <div className="error">
          ❌ {error}
        </div>
      )}
      
      {loading && (
        <div className="loading">
          <div className="spinner"></div>
          <p>Loading issues...</p>
        </div>
      )}
      
      {!loading && issues.length === 0 && (
        <div className="empty">
          <p>📋 No issues to display</p>
          <p>Click "Fetch Issues" to load data</p>
        </div>
      )}
      
      {!loading && issues.length > 0 && (
        <div className="issues-container">
          <h2>Issues List ({issues.length})</h2>
          
          <div className="issues-grid">
            {issues.map((issue) => (
              <div key={issue.id} className="issue-card">
                <div className="issue-header">
                  <span className="issue-id">#{issue.id}</span>
                  <span className={`status ${issue.status.toLowerCase().replace(' ', '-')}`}>
                    {issue.status}
                  </span>
                </div>
                
                <h3 className="issue-title">{issue.title}</h3>
                <p className="issue-description">{issue.description}</p>
                
                <div className="issue-footer">
                  <span className={`priority ${issue.priority.toLowerCase()}`}>
                    {issue.priority}
                  </span>
                  <span className="date">{issue.createdDate}</span>
                </div>
              </div>
            ))}
          </div>
        </div>
      )}
    </div>
  );
}

export default IssueTracker;
```

**CSS (IssueTracker.css):**
```css
.issue-tracker {
  max-width: 1200px;
  margin: 20px auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 30px;
}

.header h1 {
  color: #333;
  font-size: 36px;
  margin-bottom: 10px;
}

.header p {
  color: #666;
  font-size: 16px;
}

.controls {
  display: flex;
  gap: 15px;
  justify-content: center;
  margin-bottom: 30px;
}

button {
  padding: 12px 30px;
  font-size: 16px;
  font-weight: 600;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s;
}

.fetch-btn {
  background: #007bff;
  color: white;
}

.fetch-btn:hover:not(:disabled) {
  background: #0056b3;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,123,255,0.3);
}

.fetch-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.clear-btn {
  background: #dc3545;
  color: white;
}

.clear-btn:hover {
  background: #c82333;
}

.error {
  background: #f8d7da;
  color: #721c24;
  padding: 15px;
  border-radius: 8px;
  text-align: center;
  margin-bottom: 20px;
}

.loading {
  text-align: center;
  padding: 40px;
}

.spinner {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #007bff;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 1s linear infinite;
  margin: 0 auto 20px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.empty {
  text-align: center;
  padding: 60px 20px;
  background: #f8f9fa;
  border-radius: 12px;
  color: #666;
}

.empty p {
  margin: 10px 0;
  font-size: 18px;
}

.issues-container h2 {
  text-align: center;
  color: #333;
  margin-bottom: 25px;
}

.issues-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 20px;
}

.issue-card {
  background: white;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  padding: 20px;
  transition: all 0.3s;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.issue-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.15);
}

.issue-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.issue-id {
  font-weight: bold;
  color: #007bff;
  font-size: 14px;
}

.status {
  padding: 5px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
}

.status.open {
  background: #ffc107;
  color: #000;
}

.status.in-progress {
  background: #17a2b8;
  color: white;
}

.status.closed {
  background: #28a745;
  color: white;
}

.issue-title {
  color: #333;
  font-size: 18px;
  margin-bottom: 10px;
}

.issue-description {
  color: #666;
  font-size: 14px;
  line-height: 1.5;
  margin-bottom: 15px;
}

.issue-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 15px;
  border-top: 1px solid #e0e0e0;
}

.priority {
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 600;
}

.priority.high {
  background: #dc3545;
  color: white;
}

.priority.medium {
  background: #fd7e14;
  color: white;
}

.priority.low {
  background: #6c757d;
  color: white;
}

.date {
  font-size: 13px;
  color: #999;
}
```

**Explanation:**

**1. State Management:**
```javascript
const [issues, setIssues] = useState([]);  // Empty initially
const [loading, setLoading] = useState(false);  // Loading state
```

**2. Event Handler:**
```javascript
const handleFetchIssues = () => {
  setLoading(true);  // Show loading
  
  setTimeout(() => {
    setIssues(sampleIssues);  // Update state
    setLoading(false);  // Hide loading
  }, 1000);
};
```

**3. Button Click:**
```javascript
<button onClick={handleFetchIssues}>
  Fetch Issues
</button>
```

**4. Display Issues:**
```javascript
{issues.map((issue) => (
  <div key={issue.id}>
    <h3>{issue.title}</h3>
    <p>{issue.description}</p>
  </div>
))}
```

**Complete Flow:**
```
1. User clicks "Fetch Issues" button
   ↓
2. onClick triggers handleFetchIssues()
   ↓
3. setLoading(true) - shows spinner
   ↓
4. setTimeout simulates API call (1 second)
   ↓
5. setIssues(sampleIssues) - updates state
   ↓
6. setLoading(false) - hides spinner
   ↓
7. Component re-renders
   ↓
8. map() creates issue cards
   ↓
9. Issues displayed on screen
```

**Key Features:**
- ✅ Button click event handling
- ✅ State updates (useState)
- ✅ Loading state
- ✅ Error handling
- ✅ Conditional rendering
- ✅ map() for rendering lists
- ✅ Responsive design

**Remember:** Click button → Update state → Re-render → Show data

---
