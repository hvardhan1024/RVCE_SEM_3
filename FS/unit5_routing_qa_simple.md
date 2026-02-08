# Unit V - React Routing Q&A Guide (Simplified)

## 5 MARKS QUESTIONS

### 1. Explain the difference between hash-based routing and browser history routing

**Answer:**

**Hash-Based Routing (#):**
- Uses `#` symbol in URL
- Example: `http://example.com/#/home`
- URL changes but page doesn't reload
- Works on all servers (no special configuration needed)
- Uses `<HashRouter>` in React

**Browser History Routing:**
- Clean URLs without `#`
- Example: `http://example.com/home`
- Uses HTML5 History API
- Needs server configuration
- Uses `<BrowserRouter>` in React

**Examples:**

```javascript
// Hash-Based Routing
import { HashRouter } from 'react-router-dom';

<HashRouter>
  <App />
</HashRouter>
// URL: localhost:3000/#/issues
```

```javascript
// Browser History Routing
import { BrowserRouter } from 'react-router-dom';

<BrowserRouter>
  <App />
</BrowserRouter>
// URL: localhost:3000/issues (clean!)
```

**Key Difference:** Hash routing has `#`, browser history routing has clean URLs.

---

### 2. Write React Router code for basic routes with fallback

**Answer:**

```javascript
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Home route */}
        <Route path="/" element={<Home />} />
        
        {/* Issues list route */}
        <Route path="/issues" element={<IssueList />} />
        
        {/* Fallback route for invalid paths */}
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

// Components
function Home() {
  return <h1>Welcome Home!</h1>;
}

function IssueList() {
  return <h1>All Issues</h1>;
}

function NotFound() {
  return <h1>404 - Page Not Found</h1>;
}
```

**Role of `path="*"`:**
- The `*` acts as a **catch-all** route
- Matches **any URL** that doesn't match previous routes
- Should be placed **last** in the route list
- Used for showing "Page Not Found" messages

**How it works:**
1. User visits `/` → Shows Home
2. User visits `/issues` → Shows IssueList
3. User visits `/xyz` → Shows NotFound (because * catches everything else)

---

### 3. What are route parameters? Explain useParams()

**Answer:**

**Route Parameters** are **dynamic parts** of the URL used to identify specific items.

**Example Scenario:** Viewing a specific issue

```javascript
import { BrowserRouter, Routes, Route, useParams } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* :id is a route parameter (dynamic) */}
        <Route path="/issues/:id" element={<IssueDetail />} />
      </Routes>
    </BrowserRouter>
  );
}

// Component that uses the parameter
function IssueDetail() {
  // useParams() extracts the id from URL
  const { id } = useParams();
  
  return (
    <div>
      <h1>Issue Details</h1>
      <p>Viewing issue number: {id}</p>
    </div>
  );
}
```

**How it works:**
- URL: `/issues/123` → `id = "123"`
- URL: `/issues/456` → `id = "456"`
- The `:id` in route definition means "this part is dynamic"
- `useParams()` gives you the actual value from the URL

**Real Example:**
```javascript
function IssueDetail() {
  const { id } = useParams();
  const [issue, setIssue] = useState(null);
  
  useEffect(() => {
    // Fetch issue data using the id
    fetch(`/api/issues/${id}`)
      .then(res => res.json())
      .then(data => setIssue(data));
  }, [id]);
  
  return <div>{issue?.title}</div>;
}
```

**Remember:** `:id` in route = dynamic, `useParams()` = get the value

---

### 4. How to use query strings to filter data?

**Answer:**

**Query Strings** are URL parameters after `?` used for filtering/searching.

**Format:** `/issues?status=Open&priority=High`

```javascript
import { useSearchParams } from 'react-router-dom';

function IssueList() {
  // useSearchParams gives us the query string
  const [searchParams] = useSearchParams();
  
  // Read specific parameter
  const status = searchParams.get('status');
  
  return (
    <div>
      <h1>Issues</h1>
      <p>Filter: {status || 'All'}</p>
      {/* Show only issues matching the status */}
    </div>
  );
}
```

**Complete Example with Filtering:**

```javascript
import { useState, useEffect } from 'react';
import { useSearchParams } from 'react-router-dom';

function IssueList() {
  const [searchParams] = useSearchParams();
  const [issues, setIssues] = useState([]);
  
  // Get status from URL: ?status=Open
  const status = searchParams.get('status');
  
  useEffect(() => {
    // Fetch filtered issues
    const url = status 
      ? `/api/issues?status=${status}` 
      : '/api/issues';
    
    fetch(url)
      .then(res => res.json())
      .then(data => setIssues(data));
  }, [status]);
  
  return (
    <div>
      <h1>Issues ({status || 'All'})</h1>
      {issues.map(issue => (
        <div key={issue.id}>{issue.title}</div>
      ))}
    </div>
  );
}
```

**URLs and Results:**
- `/issues` → Shows all issues
- `/issues?status=Open` → Shows only open issues
- `/issues?status=Closed` → Shows only closed issues

**Key Points:**
- `searchParams.get('status')` reads the query string
- Query strings don't require route definition (they're optional)
- Good for filters, search, pagination

---

### 5. What is programmatic routing? Show useNavigate() example

**Answer:**

**Programmatic Routing** means **navigating using code** instead of clicking links.

**When to use:**
- After form submission (redirect to success page)
- After login (redirect to dashboard)
- After delete (redirect to list page)
- Based on conditions (if user not logged in, redirect to login)

**Simple Example:**

```javascript
import { useNavigate } from 'react-router-dom';

function AddIssue() {
  const navigate = useNavigate();
  
  const handleSubmit = (e) => {
    e.preventDefault();
    
    // Save the issue (API call)
    // ...
    
    // Then redirect to issues list
    navigate('/issues');
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input type="text" placeholder="Issue title" />
      <button type="submit">Add Issue</button>
    </form>
  );
}
```

**Complete Example with API:**

```javascript
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';

function AddIssue() {
  const navigate = useNavigate();
  const [title, setTitle] = useState('');
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    // Save new issue
    const response = await fetch('/api/issues', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ title })
    });
    
    if (response.ok) {
      // Success! Redirect to issues list
      navigate('/issues');
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input 
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        placeholder="Issue title"
      />
      <button type="submit">Add Issue</button>
    </form>
  );
}
```

**Different Navigate Options:**

```javascript
// Go to a page
navigate('/issues');

// Go back
navigate(-1);

// Go forward
navigate(1);

// Replace (don't add to history)
navigate('/dashboard', { replace: true });
```

**Remember:** `navigate()` = go to a page programmatically (in code)

---

### 6. What are nested routes? Show example with Outlet

**Answer:**

**Nested Routes** are routes **inside other routes** - like a layout with changing content inside.

**Think of it like:** A house (layout) with different rooms (child routes)

**Simple Example:**

```javascript
import { BrowserRouter, Routes, Route, Outlet, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Parent route with layout */}
        <Route path="/" element={<AppLayout />}>
          {/* Child routes */}
          <Route index element={<Home />} />
          <Route path="issues" element={<IssueList />} />
          <Route path="about" element={<About />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

// Layout component (stays same for all pages)
function AppLayout() {
  return (
    <div>
      {/* Navigation bar (appears on all pages) */}
      <nav>
        <Link to="/">Home</Link>
        <Link to="/issues">Issues</Link>
        <Link to="/about">About</Link>
      </nav>
      
      {/* <Outlet /> is where child routes appear */}
      <main>
        <Outlet />
      </main>
      
      {/* Footer (appears on all pages) */}
      <footer>© 2024 My App</footer>
    </div>
  );
}

function Home() {
  return <h1>Home Page</h1>;
}

function IssueList() {
  return <h1>Issues List</h1>;
}

function About() {
  return <h1>About Us</h1>;
}
```

**How it works:**
1. User visits `/` → AppLayout shows + Home appears in `<Outlet />`
2. User visits `/issues` → AppLayout shows + IssueList appears in `<Outlet />`
3. User visits `/about` → AppLayout shows + About appears in `<Outlet />`

**Visual Representation:**
```
┌─────────────────────────────┐
│  Navigation Bar (stays)     │
├─────────────────────────────┤
│                             │
│  <Outlet /> content         │ ← Changes based on route
│  (Home / Issues / About)    │
│                             │
├─────────────────────────────┤
│  Footer (stays)             │
└─────────────────────────────┘
```

**Key Points:**
- Parent route = Layout/Container
- `<Outlet />` = Placeholder for child routes
- Navigation and footer stay the same
- Only content inside `<Outlet />` changes

---

## 6 MARKS QUESTION

### Design React app with route parameters, query strings, and programmatic routing

**Answer:**

```javascript
import { BrowserRouter, Routes, Route, Link, useParams, 
         useSearchParams, useNavigate } from 'react-router-dom';
import { useState, useEffect } from 'react';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/employees" element={<EmployeeList />} />
        <Route path="/employees/:id" element={<EmployeeDetail />} />
      </Routes>
    </BrowserRouter>
  );
}

// 1. EMPLOYEE LIST with Filter Form
function EmployeeList() {
  const [searchParams] = useSearchParams();
  const navigate = useNavigate();
  const [employees, setEmployees] = useState([]);
  const [department, setDepartment] = useState('');
  
  // Read query string: ?department=HR
  const filterDepartment = searchParams.get('department');
  
  useEffect(() => {
    // Fetch employees (filtered if query string exists)
    const url = filterDepartment 
      ? `/api/employees?department=${filterDepartment}`
      : '/api/employees';
    
    fetch(url)
      .then(res => res.json())
      .then(data => setEmployees(data));
  }, [filterDepartment]);
  
  // Handle form submission
  const handleFilterSubmit = (e) => {
    e.preventDefault();
    
    // Programmatic routing to update URL with filter
    navigate(`/employees?department=${department}`);
  };
  
  return (
    <div>
      <h1>Employees</h1>
      
      {/* Filter Form */}
      <form onSubmit={handleFilterSubmit}>
        <select 
          value={department}
          onChange={(e) => setDepartment(e.target.value)}
        >
          <option value="">All Departments</option>
          <option value="HR">HR</option>
          <option value="IT">IT</option>
          <option value="Sales">Sales</option>
        </select>
        <button type="submit">Filter</button>
      </form>
      
      {/* Show current filter */}
      {filterDepartment && (
        <p>Showing: {filterDepartment} Department</p>
      )}
      
      {/* Employee Cards */}
      <div>
        {employees.map(emp => (
          <div key={emp.id}>
            <h3>{emp.name}</h3>
            <Link to={`/employees/${emp.id}`}>View Details</Link>
          </div>
        ))}
      </div>
    </div>
  );
}

// 2. EMPLOYEE DETAIL with Route Parameters
function EmployeeDetail() {
  // Get id from URL: /employees/:id
  const { id } = useParams();
  const [employee, setEmployee] = useState(null);
  
  useEffect(() => {
    // Fetch employee details using id
    fetch(`/api/employees/${id}`)
      .then(res => res.json())
      .then(data => setEmployee(data));
  }, [id]);
  
  if (!employee) {
    return <p>Loading...</p>;
  }
  
  return (
    <div>
      <h1>Employee Details</h1>
      <p><strong>ID:</strong> {employee.id}</p>
      <p><strong>Name:</strong> {employee.name}</p>
      <p><strong>Department:</strong> {employee.department}</p>
      <p><strong>Email:</strong> {employee.email}</p>
      <p><strong>Salary:</strong> ${employee.salary}</p>
      
      <Link to="/employees">Back to List</Link>
    </div>
  );
}
```

**How it works:**

**1. Route Parameters (`/employees/:id`):**
```
URL: /employees/5
Result: Shows details of employee with ID 5
How: useParams() gets the id = "5"
```

**2. Query Strings (`?department=HR`):**
```
URL: /employees?department=HR
Result: Shows only HR employees
How: useSearchParams() gets department = "HR"
```

**3. Programmatic Routing:**
```javascript
// When form is submitted:
navigate(`/employees?department=${department}`);
// This updates the URL and triggers re-render
```

**Flow:**
1. User selects "HR" from dropdown
2. Clicks "Filter" button
3. Form submission calls `navigate('/employees?department=HR')`
4. URL changes to `/employees?department=HR`
5. `useEffect` sees query string changed
6. Fetches filtered data from API
7. Shows only HR employees

**Key Concepts Used:**
- ✅ Route parameters (`:id`)
- ✅ Query strings (`?department=HR`)
- ✅ Programmatic routing (`navigate()`)
- ✅ API integration (`fetch`)

---

## 10 MARKS QUESTIONS

### 1. Movie booking app with routing, forms, and API

**Answer:**

```javascript
import { HashRouter, Routes, Route, useParams, useNavigate } from 'react-router-dom';
import { useState } from 'react';

function App() {
  return (
    <HashRouter>
      <Routes>
        <Route path="/" element={<MovieList />} />
        <Route path="/movies/:id" element={<MovieBooking />} />
        <Route path="/confirmation" element={<Confirmation />} />
      </Routes>
    </HashRouter>
  );
}

// COMPONENT 1: Movie List
function MovieList() {
  const movies = [
    { id: 1, title: 'Avengers', price: 250 },
    { id: 2, title: 'Inception', price: 200 },
    { id: 3, title: 'Interstellar', price: 220 }
  ];
  
  return (
    <div>
      <h1>Movies</h1>
      {movies.map(movie => (
        <div key={movie.id}>
          <h3>{movie.title} - ₹{movie.price}</h3>
          <a href={`#/movies/${movie.id}`}>Book Now</a>
        </div>
      ))}
    </div>
  );
}

// COMPONENT 2: Movie Booking Form
function MovieBooking() {
  const { id } = useParams(); // Get movie id from URL
  const navigate = useNavigate();
  
  // Form state
  const [formData, setFormData] = useState({
    movieId: id,
    seatNumber: '',
    customerName: '',
    phone: ''
  });
  
  const [errors, setErrors] = useState({});
  
  // Validate form
  const validate = () => {
    const newErrors = {};
    
    if (!formData.seatNumber) {
      newErrors.seatNumber = 'Seat number is required';
    } else if (!/^[A-Z]\d+$/.test(formData.seatNumber)) {
      newErrors.seatNumber = 'Invalid format. Use A1, B5, etc.';
    }
    
    if (!formData.customerName) {
      newErrors.customerName = 'Name is required';
    }
    
    if (!formData.phone) {
      newErrors.phone = 'Phone is required';
    } else if (!/^\d{10}$/.test(formData.phone)) {
      newErrors.phone = 'Phone must be 10 digits';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  // Handle form submission
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    // Validate
    if (!validate()) {
      return;
    }
    
    try {
      // POST API to save booking
      const response = await fetch('/api/bookings', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
      });
      
      if (response.ok) {
        const data = await response.json();
        
        // Programmatic redirect to confirmation page
        navigate('/confirmation', { 
          state: { bookingId: data.bookingId } 
        });
      } else {
        alert('Booking failed!');
      }
    } catch (error) {
      alert('Error: ' + error.message);
    }
  };
  
  return (
    <div>
      <h1>Book Movie (ID: {id})</h1>
      
      <form onSubmit={handleSubmit}>
        {/* Seat Number */}
        <div>
          <label>Seat Number (e.g., A5, B12):</label>
          <input
            type="text"
            value={formData.seatNumber}
            onChange={(e) => setFormData({
              ...formData,
              seatNumber: e.target.value.toUpperCase()
            })}
            placeholder="A5"
          />
          {errors.seatNumber && (
            <p style={{color: 'red'}}>{errors.seatNumber}</p>
          )}
        </div>
        
        {/* Customer Name */}
        <div>
          <label>Your Name:</label>
          <input
            type="text"
            value={formData.customerName}
            onChange={(e) => setFormData({
              ...formData,
              customerName: e.target.value
            })}
          />
          {errors.customerName && (
            <p style={{color: 'red'}}>{errors.customerName}</p>
          )}
        </div>
        
        {/* Phone */}
        <div>
          <label>Phone:</label>
          <input
            type="tel"
            value={formData.phone}
            onChange={(e) => setFormData({
              ...formData,
              phone: e.target.value
            })}
            placeholder="1234567890"
          />
          {errors.phone && (
            <p style={{color: 'red'}}>{errors.phone}</p>
          )}
        </div>
        
        <button type="submit">Confirm Booking</button>
      </form>
    </div>
  );
}

// COMPONENT 3: Confirmation Page
function Confirmation() {
  const navigate = useNavigate();
  
  return (
    <div>
      <h1>✓ Booking Confirmed!</h1>
      <p>Your ticket has been booked successfully.</p>
      <button onClick={() => navigate('/')}>
        Back to Movies
      </button>
    </div>
  );
}
```

**Explanation:**

**1. Hash-Based Routing:**
```javascript
<HashRouter>  // Uses # in URLs
  // URL: localhost:3000/#/movies/1
</HashRouter>
```

**2. Route Parameters:**
```javascript
<Route path="/movies/:id" />  // :id is dynamic
const { id } = useParams();    // Get the id value
```

**3. Form Validation:**
```javascript
// Seat number must be like A5, B12
if (!/^[A-Z]\d+$/.test(formData.seatNumber)) {
  errors.seatNumber = 'Invalid format';
}

// Phone must be 10 digits
if (!/^\d{10}$/.test(formData.phone)) {
  errors.phone = 'Phone must be 10 digits';
}
```

**4. POST API:**
```javascript
fetch('/api/bookings', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(formData)
});
```

**5. Programmatic Redirect:**
```javascript
navigate('/confirmation', { 
  state: { bookingId: data.bookingId } 
});
```

**User Flow:**
1. Select movie from list (click "Book Now")
2. URL becomes `#/movies/1`
3. Fill booking form (seat, name, phone)
4. Click "Confirm Booking"
5. Form validates (seat format, phone digits)
6. POST API saves booking
7. Redirects to `/confirmation`
8. Shows success message

---

### 2. Dashboard with nested routes and query strings

**Answer:**

```javascript
import { BrowserRouter, Routes, Route, Outlet, Link, 
         useSearchParams } from 'react-router-dom';
import { useState, useEffect } from 'react';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Parent route */}
        <Route path="/dashboard" element={<DashboardLayout />}>
          {/* Child routes */}
          <Route index element={<DashboardHome />} />
          <Route path="reports" element={<ReportsList />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

// LAYOUT: Dashboard (appears on all dashboard pages)
function DashboardLayout() {
  return (
    <div>
      {/* Sidebar navigation */}
      <nav style={{ background: '#333', color: 'white', padding: '20px' }}>
        <h2>Dashboard</h2>
        <ul>
          <li><Link to="/dashboard">Home</Link></li>
          <li><Link to="/dashboard/reports">Reports</Link></li>
        </ul>
      </nav>
      
      {/* Main content area - child routes appear here */}
      <main style={{ padding: '20px' }}>
        <Outlet />  {/* Child routes render here */}
      </main>
    </div>
  );
}

// CHILD ROUTE 1: Dashboard Home
function DashboardHome() {
  return (
    <div>
      <h1>Dashboard Home</h1>
      <p>Welcome to your dashboard!</p>
    </div>
  );
}

// CHILD ROUTE 2: Reports List with Query String Filtering
function ReportsList() {
  const [searchParams, setSearchParams] = useSearchParams();
  const [reports, setReports] = useState([]);
  const [loading, setLoading] = useState(true);
  
  // Read query string: ?type=monthly
  const reportType = searchParams.get('type') || 'all';
  
  // Fetch reports when filter changes
  useEffect(() => {
    setLoading(true);
    
    // Build API URL with query string
    const url = reportType !== 'all'
      ? `/api/reports?type=${reportType}`
      : '/api/reports';
    
    fetch(url)
      .then(res => res.json())
      .then(data => {
        setReports(data);
        setLoading(false);
      })
      .catch(error => {
        console.error('Error:', error);
        setLoading(false);
      });
  }, [reportType]);
  
  // Update filter (changes URL query string)
  const handleFilterChange = (type) => {
    if (type === 'all') {
      setSearchParams({});  // Remove query string
    } else {
      setSearchParams({ type });  // Set ?type=monthly
    }
  };
  
  return (
    <div>
      <h1>Reports</h1>
      
      {/* Filter Buttons */}
      <div style={{ marginBottom: '20px' }}>
        <button 
          onClick={() => handleFilterChange('all')}
          style={{ 
            background: reportType === 'all' ? 'blue' : 'gray',
            color: 'white',
            margin: '5px'
          }}
        >
          All
        </button>
        
        <button 
          onClick={() => handleFilterChange('monthly')}
          style={{ 
            background: reportType === 'monthly' ? 'blue' : 'gray',
            color: 'white',
            margin: '5px'
          }}
        >
          Monthly
        </button>
        
        <button 
          onClick={() => handleFilterChange('quarterly')}
          style={{ 
            background: reportType === 'quarterly' ? 'blue' : 'gray',
            color: 'white',
            margin: '5px'
          }}
        >
          Quarterly
        </button>
        
        <button 
          onClick={() => handleFilterChange('yearly')}
          style={{ 
            background: reportType === 'yearly' ? 'blue' : 'gray',
            color: 'white',
            margin: '5px'
          }}
        >
          Yearly
        </button>
      </div>
      
      {/* Current Filter Display */}
      <p>Showing: <strong>{reportType}</strong> reports</p>
      
      {/* Reports List */}
      {loading ? (
        <p>Loading reports...</p>
      ) : (
        <div>
          {reports.length === 0 ? (
            <p>No reports found.</p>
          ) : (
            reports.map(report => (
              <div key={report.id} style={{ 
                border: '1px solid #ccc', 
                padding: '10px', 
                margin: '10px 0' 
              }}>
                <h3>{report.title}</h3>
                <p>Type: {report.type}</p>
                <p>Date: {report.date}</p>
                <p>{report.description}</p>
              </div>
            ))
          )}
        </div>
      )}
    </div>
  );
}
```

**Explanation:**

**1. Nested Routes Structure:**
```
/dashboard                    → DashboardLayout (with sidebar)
  ├─ index                    → DashboardHome (in <Outlet />)
  └─ /dashboard/reports       → ReportsList (in <Outlet />)
```

**2. How `<Outlet />` Works:**
```javascript
function DashboardLayout() {
  return (
    <div>
      <nav>Sidebar (stays same)</nav>
      <main>
        <Outlet />  {/* Child content appears here */}
      </main>
    </div>
  );
}
```

**3. Query String Filtering:**
```
URL: /dashboard/reports
Shows: All reports

URL: /dashboard/reports?type=monthly
Shows: Only monthly reports

URL: /dashboard/reports?type=quarterly
Shows: Only quarterly reports
```

**4. How Query String is Read:**
```javascript
const [searchParams] = useSearchParams();
const reportType = searchParams.get('type');
// If URL is /reports?type=monthly, reportType = "monthly"
```

**5. How Query String is Updated:**
```javascript
// Set query string
setSearchParams({ type: 'monthly' });
// URL becomes: /dashboard/reports?type=monthly

// Remove query string
setSearchParams({});
// URL becomes: /dashboard/reports
```

**Visual Flow:**
```
┌─────────────────────────────────────┐
│  Sidebar (DashboardLayout)          │
│  - Dashboard Home                   │
│  - Reports                          │
├─────────────────────────────────────┤
│  <Outlet /> renders:                │
│                                     │
│  When /dashboard:                   │
│    → DashboardHome component        │
│                                     │
│  When /dashboard/reports:           │
│    → ReportsList component          │
│                                     │
│  When /dashboard/reports?type=...:  │
│    → ReportsList with filter        │
└─────────────────────────────────────┘
```

**Key Concepts:**
- ✅ Nested routes (parent/child)
- ✅ `<Outlet />` for child rendering
- ✅ Query strings for filtering
- ✅ Dynamic URL updates
- ✅ API integration with filters

---

### 3. Edit page with route parameters and programmatic routing

**Answer:**

```javascript
import { BrowserRouter, Routes, Route, useParams, useNavigate } from 'react-router-dom';
import { useState, useEffect } from 'react';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/issues" element={<IssuesList />} />
        <Route path="/issues/:id/edit" element={<EditIssuePage />} />
      </Routes>
    </BrowserRouter>
  );
}

// COMPONENT 1: Issues List
function IssuesList() {
  const [searchParams] = useSearchParams();
  const [issues, setIssues] = useState([]);
  
  const status = searchParams.get('status');
  
  useEffect(() => {
    fetch('/api/issues')
      .then(res => res.json())
      .then(data => setIssues(data));
  }, []);
  
  return (
    <div>
      <h1>Issues</h1>
      {status && <p>Status: {status}</p>}
      
      {issues.map(issue => (
        <div key={issue.id}>
          <h3>{issue.title}</h3>
          <p>Status: {issue.status}</p>
          <Link to={`/issues/${issue.id}/edit`}>Edit</Link>
        </div>
      ))}
    </div>
  );
}

// COMPONENT 2: Edit Issue Page
function EditIssuePage() {
  // Get issue id from URL: /issues/:id/edit
  const { id } = useParams();
  const navigate = useNavigate();
  
  // Form state
  const [formData, setFormData] = useState({
    title: '',
    description: '',
    status: 'Open'
  });
  
  const [loading, setLoading] = useState(true);
  const [errors, setErrors] = useState({});
  
  // Load issue details when component mounts
  useEffect(() => {
    // Fetch issue data using id from useParams()
    fetch(`/api/issues/${id}`)
      .then(res => res.json())
      .then(data => {
        setFormData({
          title: data.title,
          description: data.description,
          status: data.status
        });
        setLoading(false);
      })
      .catch(error => {
        console.error('Error loading issue:', error);
        setLoading(false);
      });
  }, [id]);
  
  // Validate form
  const validate = () => {
    const newErrors = {};
    
    if (!formData.title || formData.title.trim() === '') {
      newErrors.title = 'Title is required';
    }
    
    if (!formData.description || formData.description.trim() === '') {
      newErrors.description = 'Description is required';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  // Handle form submission
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    // Validate
    if (!validate()) {
      return;
    }
    
    try {
      // PUT API to update issue
      const response = await fetch(`/api/issues/${id}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
      });
      
      if (response.ok) {
        // Success! Programmatically redirect to issues list
        // with status query string
        navigate('/issues?status=Updated');
      } else {
        alert('Failed to update issue');
      }
    } catch (error) {
      alert('Error: ' + error.message);
    }
  };
  
  // Handle input changes
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData({
      ...formData,
      [name]: value
    });
    
    // Clear error for this field
    if (errors[name]) {
      setErrors({
        ...errors,
        [name]: null
      });
    }
  };
  
  // Show loading state
  if (loading) {
    return <p>Loading issue details...</p>;
  }
  
  return (
    <div>
      <h1>Edit Issue #{id}</h1>
      
      <form onSubmit={handleSubmit}>
        {/* Title */}
        <div>
          <label>Title:</label>
          <input
            type="text"
            name="title"
            value={formData.title}
            onChange={handleChange}
            style={{ 
              border: errors.title ? '2px solid red' : '1px solid #ccc',
              width: '100%',
              padding: '8px'
            }}
          />
          {errors.title && (
            <p style={{ color: 'red' }}>{errors.title}</p>
          )}
        </div>
        
        {/* Description */}
        <div>
          <label>Description:</label>
          <textarea
            name="description"
            value={formData.description}
            onChange={handleChange}
            rows="5"
            style={{ 
              border: errors.description ? '2px solid red' : '1px solid #ccc',
              width: '100%',
              padding: '8px'
            }}
          />
          {errors.description && (
            <p style={{ color: 'red' }}>{errors.description}</p>
          )}
        </div>
        
        {/* Status */}
        <div>
          <label>Status:</label>
          <select
            name="status"
            value={formData.status}
            onChange={handleChange}
            style={{ padding: '8px' }}
          >
            <option value="Open">Open</option>
            <option value="In Progress">In Progress</option>
            <option value="Resolved">Resolved</option>
            <option value="Closed">Closed</option>
          </select>
        </div>
        
        {/* Buttons */}
        <div style={{ marginTop: '20px' }}>
          <button type="submit" style={{ 
            background: 'blue', 
            color: 'white',
            padding: '10px 20px',
            marginRight: '10px'
          }}>
            Save Changes
          </button>
          
          <button 
            type="button"
            onClick={() => navigate('/issues')}
            style={{ 
              background: 'gray', 
              color: 'white',
              padding: '10px 20px'
            }}
          >
            Cancel
          </button>
        </div>
      </form>
    </div>
  );
}
```

**Explanation:**

**1. Route Parameters (`:id`):**
```javascript
// Route definition
<Route path="/issues/:id/edit" element={<EditIssuePage />} />

// In component
const { id } = useParams();  // Gets "5" from /issues/5/edit
```

**2. Loading Issue Details:**
```javascript
useEffect(() => {
  fetch(`/api/issues/${id}`)  // Use id from URL
    .then(res => res.json())
    .then(data => {
      setFormData({
        title: data.title,        // Pre-fill form
        description: data.description,
        status: data.status
      });
    });
}, [id]);
```

**3. PUT API to Update:**
```javascript
fetch(`/api/issues/${id}`, {
  method: 'PUT',              // Update method
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(formData)
});
```

**4. Programmatic Redirect with Query String:**
```javascript
// After successful update
navigate('/issues?status=Updated');
// Goes to: /issues?status=Updated
```

**User Flow:**
1. User clicks "Edit" on issue #5
2. URL becomes `/issues/5/edit`
3. `useParams()` extracts `id = "5"`
4. Fetch issue #5 details from API
5. Pre-fill form with existing data
6. User edits title/description/status
7. Clicks "Save Changes"
8. Validates form (title & description required)
9. PUT API updates issue #5
10. Redirects to `/issues?status=Updated`
11. Issues list shows "Status: Updated" message

**Key Concepts:**
- ✅ Route parameters (`:id`)
- ✅ useParams() to get id
- ✅ useEffect() to load data
- ✅ Form validation
- ✅ PUT API for updates
- ✅ Programmatic routing with query string

---

### 9. Simple routing with edit page and validation

**Answer:**

```javascript
import { BrowserRouter, Routes, Route, Link, useParams, useNavigate } from 'react-router-dom';
import { useState, useEffect } from 'react';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/books" element={<BooksList />} />
        <Route path="/books/:id/edit" element={<EditBook />} />
      </Routes>
    </BrowserRouter>
  );
}

// COMPONENT 1: Books List
function BooksList() {
  const [books, setBooks] = useState([]);
  
  useEffect(() => {
    // Fetch all books
    fetch('/api/books')
      .then(res => res.json())
      .then(data => setBooks(data));
  }, []);
  
  return (
    <div>
      <h1>Books Library</h1>
      
      {books.map(book => (
        <div key={book.id} style={{ 
          border: '1px solid #ccc', 
          padding: '10px', 
          margin: '10px 0' 
        }}>
          <h3>{book.title}</h3>
          <p>Author: {book.author}</p>
          <p>Price: ${book.price}</p>
          
          <Link to={`/books/${book.id}/edit`}>
            <button>Edit Book</button>
          </Link>
        </div>
      ))}
    </div>
  );
}

// COMPONENT 2: Edit Book Page
function EditBook() {
  const { id } = useParams();  // Get book id from URL
  const navigate = useNavigate();
  
  const [formData, setFormData] = useState({
    title: '',
    author: '',
    price: '',
    isbn: ''
  });
  
  const [errors, setErrors] = useState({});
  const [loading, setLoading] = useState(true);
  
  // Load book details
  useEffect(() => {
    fetch(`/api/books/${id}`)
      .then(res => res.json())
      .then(data => {
        // Pre-fill form with existing book data
        setFormData({
          title: data.title,
          author: data.author,
          price: data.price,
          isbn: data.isbn
        });
        setLoading(false);
      })
      .catch(error => {
        console.error('Error:', error);
        setLoading(false);
      });
  }, [id]);
  
  // Validate form
  const validate = () => {
    const newErrors = {};
    
    // Title validation
    if (!formData.title || formData.title.trim() === '') {
      newErrors.title = 'Title cannot be empty';
    } else if (formData.title.length < 3) {
      newErrors.title = 'Title must be at least 3 characters';
    }
    
    // Author validation
    if (!formData.author || formData.author.trim() === '') {
      newErrors.author = 'Author cannot be empty';
    }
    
    // Price validation
    if (!formData.price) {
      newErrors.price = 'Price is required';
    } else if (isNaN(formData.price) || Number(formData.price) <= 0) {
      newErrors.price = 'Price must be a positive number';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  // Handle input changes
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData({
      ...formData,
      [name]: value
    });
  };
  
  // Handle form submission
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    // Validate before submitting
    if (!validate()) {
      alert('Please fix the errors before submitting');
      return;
    }
    
    try {
      // PUT API to update book
      const response = await fetch(`/api/books/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          title: formData.title.trim(),
          author: formData.author.trim(),
          price: Number(formData.price),
          isbn: formData.isbn
        })
      });
      
      if (response.ok) {
        // Success! Redirect to books list
        navigate('/books');
      } else {
        const error = await response.json();
        alert('Update failed: ' + error.message);
      }
    } catch (error) {
      console.error('Error updating book:', error);
      alert('Failed to update book');
    }
  };
  
  if (loading) {
    return <p>Loading book details...</p>;
  }
  
  return (
    <div style={{ maxWidth: '600px', margin: '20px auto' }}>
      <h1>Edit Book</h1>
      
      <form onSubmit={handleSubmit}>
        {/* Title Field */}
        <div style={{ marginBottom: '15px' }}>
          <label style={{ display: 'block', marginBottom: '5px' }}>
            Title: <span style={{ color: 'red' }}>*</span>
          </label>
          <input
            type="text"
            name="title"
            value={formData.title}
            onChange={handleChange}
            style={{
              width: '100%',
              padding: '8px',
              border: errors.title ? '2px solid red' : '1px solid #ccc',
              borderRadius: '4px'
            }}
            placeholder="Enter book title"
          />
          {errors.title && (
            <p style={{ color: 'red', fontSize: '14px', marginTop: '5px' }}>
              {errors.title}
            </p>
          )}
        </div>
        
        {/* Author Field */}
        <div style={{ marginBottom: '15px' }}>
          <label style={{ display: 'block', marginBottom: '5px' }}>
            Author: <span style={{ color: 'red' }}>*</span>
          </label>
          <input
            type="text"
            name="author"
            value={formData.author}
            onChange={handleChange}
            style={{
              width: '100%',
              padding: '8px',
              border: errors.author ? '2px solid red' : '1px solid #ccc',
              borderRadius: '4px'
            }}
            placeholder="Enter author name"
          />
          {errors.author && (
            <p style={{ color: 'red', fontSize: '14px', marginTop: '5px' }}>
              {errors.author}
            </p>
          )}
        </div>
        
        {/* Price Field */}
        <div style={{ marginBottom: '15px' }}>
          <label style={{ display: 'block', marginBottom: '5px' }}>
            Price: <span style={{ color: 'red' }}>*</span>
          </label>
          <input
            type="number"
            name="price"
            value={formData.price}
            onChange={handleChange}
            step="0.01"
            style={{
              width: '100%',
              padding: '8px',
              border: errors.price ? '2px solid red' : '1px solid #ccc',
              borderRadius: '4px'
            }}
            placeholder="0.00"
          />
          {errors.price && (
            <p style={{ color: 'red', fontSize: '14px', marginTop: '5px' }}>
              {errors.price}
            </p>
          )}
        </div>
        
        {/* ISBN Field */}
        <div style={{ marginBottom: '15px' }}>
          <label style={{ display: 'block', marginBottom: '5px' }}>
            ISBN:
          </label>
          <input
            type="text"
            name="isbn"
            value={formData.isbn}
            onChange={handleChange}
            style={{
              width: '100%',
              padding: '8px',
              border: '1px solid #ccc',
              borderRadius: '4px'
            }}
            placeholder="978-3-16-148410-0"
          />
        </div>
        
        {/* Buttons */}
        <div style={{ display: 'flex', gap: '10px' }}>
          <button 
            type="submit"
            style={{
              background: '#007bff',
              color: 'white',
              padding: '10px 20px',
              border: 'none',
              borderRadius: '4px',
              cursor: 'pointer'
            }}
          >
            Update Book
          </button>
          
          <button 
            type="button"
            onClick={() => navigate('/books')}
            style={{
              background: '#6c757d',
              color: 'white',
              padding: '10px 20px',
              border: 'none',
              borderRadius: '4px',
              cursor: 'pointer'
            }}
          >
            Cancel
          </button>
        </div>
      </form>
    </div>
  );
}
```

**Validation Rules:**

1. **Title Validation:**
   - Cannot be empty
   - Must be at least 3 characters long
   
2. **Author Validation:**
   - Cannot be empty
   
3. **Price Validation:**
   - Required field
   - Must be a number
   - Must be greater than 0

**How it Works:**

**Step 1: Load Book Data**
```javascript
useEffect(() => {
  fetch(`/api/books/${id}`)
    .then(res => res.json())
    .then(data => setFormData(data));
}, [id]);
```

**Step 2: User Edits Form**
```javascript
handleChange = (e) => {
  setFormData({ ...formData, [e.target.name]: e.target.value });
}
```

**Step 3: Validate on Submit**
```javascript
if (!formData.title.trim()) {
  errors.title = 'Title cannot be empty';
}
```

**Step 4: PUT API Update**
```javascript
fetch(`/api/books/${id}`, {
  method: 'PUT',
  body: JSON.stringify(formData)
});
```

**Step 5: Redirect**
```javascript
navigate('/books');  // Go back to list
```

**User Flow:**
1. View books list at `/books`
2. Click "Edit Book" on any book
3. Goes to `/books/5/edit` (for book #5)
4. Form loads with existing book data
5. User changes title/author/price
6. Clicks "Update Book"
7. Validation checks:
   - Title not empty? ✓
   - Author not empty? ✓
   - Price > 0? ✓
8. If valid: PUT API updates book
9. Redirects to `/books`
10. If invalid: Shows error messages in red

**Key Features:**
- ✅ Pre-filled form with existing data
- ✅ Real-time validation
- ✅ Clear error messages
- ✅ PUT API for updates
- ✅ Programmatic redirect after success
- ✅ Cancel button to go back

---

## ADDITIONAL 5 MARKS QUESTIONS

### 1. Explain React Router and compare BrowserRouter vs HashRouter

**Answer:**

**React Router** is a library for handling navigation in React apps (moving between pages without refreshing).

**Why Use React Router?**
- Create Single Page Applications (SPAs)
- Navigate without page reload
- Manage URL and browser history
- Show different components based on URL

**BrowserRouter vs HashRouter:**

| Feature | BrowserRouter | HashRouter |
|---------|--------------|------------|
| URL Format | Clean: `/about` | Hash: `/#/about` |
| Uses | HTML5 History API | URL Hash |
| Server Config | Needs server setup | Works anywhere |
| Best For | Production apps | Simple demos |
| Example | `example.com/products` | `example.com/#/products` |

**BrowserRouter Example:**
```javascript
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
// URLs: localhost:3000/
//       localhost:3000/about
```

**HashRouter Example:**
```javascript
import { HashRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <HashRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </HashRouter>
  );
}
// URLs: localhost:3000/#/
//       localhost:3000/#/about
```

**When to Use:**
- **BrowserRouter**: Most production apps (cleaner URLs)
- **HashRouter**: Quick demos, GitHub Pages, older browsers

---

### 2. How to navigate in React Router? Explain useNavigate()

**Answer:**

**Two Ways to Navigate:**

**1. Using `<Link>` (for user clicks):**
```javascript
import { Link } from 'react-router-dom';

function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
      <Link to="/contact">Contact</Link>
    </nav>
  );
}
```

**2. Using `useNavigate()` (programmatic - in code):**
```javascript
import { useNavigate } from 'react-router-dom';

function LoginForm() {
  const navigate = useNavigate();
  
  const handleLogin = () => {
    // After successful login
    navigate('/dashboard');
  };
  
  return (
    <button onClick={handleLogin}>Login</button>
  );
}
```

**Push vs Replace:**

**Push (default)** - Adds to history (can go back):
```javascript
navigate('/dashboard');  // Can press back button
```

**Replace** - Replaces current page (can't go back):
```javascript
navigate('/dashboard', { replace: true });  // Can't go back
```

**Example Explaining Push:**
```javascript
function Navigation() {
  const navigate = useNavigate();
  
  return (
    <div>
      {/* Push - adds to history */}
      <button onClick={() => navigate('/page1')}>
        Go to Page 1 (can go back)
      </button>
      
      {/* Replace - replaces in history */}
      <button onClick={() => navigate('/page2', { replace: true })}>
        Go to Page 2 (can't go back)
      </button>
      
      {/* Go back */}
      <button onClick={() => navigate(-1)}>
        Back
      </button>
      
      {/* Go forward */}
      <button onClick={() => navigate(1)}>
        Forward
      </button>
    </div>
  );
}
```

**When to Use Replace:**
- After login (don't let user go back to login)
- After successful payment
- After logout
- Redirect from old URL

**Remember:**
- `Link` = for navigation buttons/menu
- `navigate()` = for programmatic navigation (after actions)
- `replace: true` = can't go back
- `navigate(-1)` = go back one page

---

### 3. Differentiate between server-side and client-side routing

**Answer:**

**Server-Side Routing (Traditional):**
- Every URL request goes to **server**
- Server sends a **new HTML page**
- Page **reloads completely**
- Used in: PHP, JSP, traditional websites

**Client-Side Routing (React Router):**
- Navigation happens in **browser only**
- **No page reload**
- Just swaps React components
- Used in: Single Page Applications (SPAs)

**Comparison Table:**

| Aspect | Server-Side | Client-Side (React) |
|--------|-------------|-------------------|
| Page Reload | Yes, every time | No |
| Speed | Slower | Faster |
| Server Request | Every navigation | Only initial load |
| User Experience | Page flickers | Smooth |
| SEO | Better | Needs extra work |
| Example | Traditional websites | React apps |

**Server-Side Routing Example:**
```
User clicks "About"
  ↓
Browser requests example.com/about from server
  ↓
Server sends new about.html
  ↓
Page reloads completely
  ↓
Shows About page
```

**Client-Side Routing (React Router):**
```
User clicks "About"
  ↓
React Router changes URL to /about
  ↓
No server request
  ↓
Just shows <About /> component
  ↓
Smooth, no reload!
```

**How React Router Implements Client-Side Routing:**
```javascript
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>  {/* Handles browser history */}
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

**What Happens:**
1. React loads once (initial page load)
2. User clicks link
3. URL changes (browser history API)
4. React shows different component
5. **No server request, no reload!**

**Advantages of Client-Side:**
- ✅ Faster navigation
- ✅ Better user experience
- ✅ Less server load
- ✅ Feels like a native app

**Disadvantages:**
- ❌ Initial load can be slower
- ❌ SEO challenges (solved with SSR)
- ❌ Requires JavaScript

---

### 4. Explain `<Route>` and useParams() with example

**Answer:**

**`<Route>` Component:**
The `<Route>` component defines **which component to show** for a specific URL.

**Basic Syntax:**
```javascript
<Route path="/about" element={<About />} />
```
Meaning: When URL is `/about`, show `<About />` component

**URL Parameters with `:id`:**
```javascript
<Route path="/users/:id" element={<UserProfile />} />
```
The `:id` makes that part **dynamic** (can be any value)

**Complete Example:**
```javascript
import { BrowserRouter, Routes, Route, useParams, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Static route */}
        <Route path="/" element={<Home />} />
        
        {/* Dynamic route with parameter */}
        <Route path="/users/:id" element={<UserProfile />} />
      </Routes>
    </BrowserRouter>
  );
}

// Home component with links
function Home() {
  return (
    <div>
      <h1>Users</h1>
      <Link to="/users/1">User 1</Link>
      <Link to="/users/2">User 2</Link>
      <Link to="/users/3">User 3</Link>
    </div>
  );
}

// UserProfile component using useParams()
function UserProfile() {
  // useParams() extracts the :id from URL
  const { id } = useParams();
  
  return (
    <div>
      <h1>User Profile</h1>
      <p>Viewing user with ID: {id}</p>
    </div>
  );
}
```

**How It Works:**
```
Click "User 1"
  ↓
URL becomes: /users/1
  ↓
Route matches: /users/:id
  ↓
useParams() returns: { id: "1" }
  ↓
Shows: "Viewing user with ID: 1"
```

**Multiple Parameters:**
```javascript
// Route with multiple params
<Route path="/posts/:postId/comments/:commentId" element={<Comment />} />

// In component
function Comment() {
  const { postId, commentId } = useParams();
  
  return (
    <div>
      <p>Post: {postId}</p>
      <p>Comment: {commentId}</p>
    </div>
  );
}

// URL: /posts/10/comments/5
// Result: postId = "10", commentId = "5"
```

**Real World Example:**
```javascript
function ProductDetail() {
  const { id } = useParams();
  const [product, setProduct] = useState(null);
  
  useEffect(() => {
    // Fetch product using id from URL
    fetch(`/api/products/${id}`)
      .then(res => res.json())
      .then(data => setProduct(data));
  }, [id]);
  
  return (
    <div>
      {product && (
        <>
          <h1>{product.name}</h1>
          <p>Price: ${product.price}</p>
        </>
      )}
    </div>
  );
}
```

**Remember:**
- `:parameter` in route = dynamic part
- `useParams()` = get the value
- Returns object: `{ id: "value" }`

---

### 10. Differentiate between Controlled and Uncontrolled components

**Answer:**

**Controlled Components:**
- React **controls** the input value
- Value stored in **state**
- Changes handled by **setState**
- **Recommended** by React

**Uncontrolled Components:**
- DOM **controls** the input value
- Value accessed using **ref**
- React doesn't track changes
- Less common

**Comparison Table:**

| Feature | Controlled | Uncontrolled |
|---------|-----------|-------------|
| Value Source | React state | DOM |
| Access Value | `state.value` | `ref.current.value` |
| Updates | onChange + setState | Direct DOM |
| Validation | Easy (in React) | Harder |
| React Way | ✅ Recommended | ❌ Not recommended |

**Controlled Component Example:**
```javascript
import { useState } from 'react';

function ControlledForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Name:', name);
    console.log('Email:', email);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {/* React controls these values */}
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Name"
      />
      
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Uncontrolled Component Example:**
```javascript
import { useRef } from 'react';

function UncontrolledForm() {
  const nameRef = useRef();
  const emailRef = useRef();
  
  const handleSubmit = (e) => {
    e.preventDefault();
    // Access values from DOM directly
    console.log('Name:', nameRef.current.value);
    console.log('Email:', emailRef.current.value);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {/* DOM controls these values */}
      <input
        type="text"
        ref={nameRef}
        placeholder="Name"
      />
      
      <input
        type="email"
        ref={emailRef}
        placeholder="Email"
      />
      
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Why Controlled is Preferred:**

**1. Easy Validation:**
```javascript
const [price, setPrice] = useState('');

const handlePriceChange = (e) => {
  const value = e.target.value;
  
  // Validate while typing
  if (value === '' || Number(value) >= 0) {
    setPrice(value);
  }
  // Negative numbers not allowed!
};

<input
  value={price}
  onChange={handlePriceChange}
  type="number"
/>
```

**2. Real-time Formatting:**
```javascript
const [phone, setPhone] = useState('');

const handlePhoneChange = (e) => {
  let value = e.target.value.replace(/\D/g, ''); // Only digits
  setPhone(value);
};

// User types "abc123" → Only "123" is saved
```

**3. Show Live Preview:**
```javascript
const [message, setMessage] = useState('');

return (
  <div>
    <textarea
      value={message}
      onChange={(e) => setMessage(e.target.value)}
    />
    
    {/* Live character count */}
    <p>{message.length} characters</p>
  </div>
);
```

**When to Use Uncontrolled:**
- File inputs (can't be controlled)
- Simple forms without validation
- Integrating with non-React code

**Example: File Input (Must be Uncontrolled):**
```javascript
function FileUpload() {
  const fileRef = useRef();
  
  const handleSubmit = (e) => {
    e.preventDefault();
    const file = fileRef.current.files[0];
    console.log('Selected file:', file);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {/* File inputs are always uncontrolled */}
      <input type="file" ref={fileRef} />
      <button type="submit">Upload</button>
    </form>
  );
}
```

**Remember:**
- **Controlled** = React state controls value (recommended!)
- **Uncontrolled** = DOM controls value (rare cases)
- For validation, always use controlled

---

