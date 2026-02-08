# Unit V – Working with React Router and Forms

> **Scope:** Routing Techniques, Simple Routing, Route Parameters, Route Query String, Programmatic Routing, Nested Routes, Browser History, Forms, Filter Form, GET API, Edit Page, UI Components, Update API, Delete API.

---

## 1) Routing Techniques

**Definition:** Techniques used by SPAs to change views by updating the URL without full page reloads.

**Key points:**

- ⭐ Modern SPAs use **client-side routing** (URL changes handled by JavaScript).
- A routing library (e.g., `react-router-dom`) maps URLs to components.
- Benefits: faster navigation, better UX, no full page reload.
- **Hash-based routing (`#`)**: browser never sends hash to server; JS listens for hash changes.
- **History API routing**: uses `pushState` / `replaceState` for clean URLs.
- ⚠️ History API requires backend to return SPA entry for all routes.

**Important terms / keywords:**

- Client-side routing, Hash routing, History API, `pushState`, `replaceState`

**Example (URL patterns):**

- Hash: `https://example.com/#/products/42`
- History API: `https://example.com/products/42`

**Server-side vs Client-side routing (exam contrast):**

| Feature        | Server-Side Routing                     | Client-Side Routing                   |
| -------------- | --------------------------------------- | ------------------------------------- |
| Mechanism      | Each navigation requests server         | JS intercepts and swaps components    |
| Performance    | Faster initial load, slower transitions | Slower initial load, fast transitions |
| SEO            | Excellent (pre-rendered HTML)           | Limited unless pre-rendered           |
| Resource usage | Higher server load                      | Lower server load                     |
| Best for       | Static sites, SEO-heavy pages           | Web apps, dashboards                  |

---

## 2) Simple Routing

**Definition:** Defining main routes that map URL paths to components.

**Key points:**

- `<Routes>` is the container for route definitions.
- `<Route path="/" element={...} />` maps a path to a component.
- `path="*"` handles invalid routes (404).

**Important terms / keywords:**

- Routes, Route, path, element, wildcard route

**Minimal example:**

```jsx
import { Routes, Route } from "react-router-dom"
import { IssueList } from "./IssueList"
import { NotFound } from "./NotFound"

export const PageRouter = () => (
  <Routes>
    <Route path="/" element={<IssueList />} />
    <Route path="/issues" element={<IssueList />} />
    <Route path="*" element={<NotFound />} />
  </Routes>
)
```

---

## 3) Route Parameters

**Definition:** Dynamic path segments used to identify specific resources.

**Key points:**

- `:id` declares a route parameter.
- `useParams()` reads parameters from URL.
- Used for detail/edit pages of a single resource.

**Important terms / keywords:**

- Route params, dynamic segments, `useParams`

**Minimal example:**

```jsx
// URL: /issues/12345
import { useParams } from "react-router-dom"

export const IssueDetail = () => {
  const { id } = useParams()
  return <h2>Displaying details for Issue ID: {id}</h2>
}

// Route:
;<Route path="/issues/:id" element={<IssueDetail />} />
```

---

## 4) Route Query String

**Definition:** Optional key-value data appended to the URL for filtering/searching.

**Key points:**

- Format: `?key=value` pairs.
- `useSearchParams()` reads query values.
- Used for filters such as status, sort, search.

**Important terms / keywords:**

- Query string, query params, `useSearchParams`

**Minimal example:**

```jsx
// URL: /issues?status=Open
import { useSearchParams } from "react-router-dom"

export const IssueList = () => {
  const [searchParams] = useSearchParams()
  const status = searchParams.get("status") || "All"
  return <div>Showing {status} Issues</div>
}
```

**Route Params vs Query Params (table):**

| Feature     | Route Parameters         | Query Params              |
| ----------- | ------------------------ | ------------------------- |
| Requirement | Mandatory; part of path  | Optional; appended string |
| Primary use | Identify single resource | Filter / sort / search    |
| Syntax      | `/issues/123`            | `/issues?status=Open`     |
| Structure   | Hierarchical path        | Modifies view/response    |

---

## 5) Programmatic Routing

**Definition:** Navigation triggered by code after an event (e.g., submit).

**Key points:**

- `useNavigate()` provides `navigate()`.
- ⭐ `navigate('/path')` adds a history entry.
- ⭐ `navigate('/path', { replace: true })` replaces current entry.

**Important terms / keywords:**

- Programmatic navigation, `useNavigate`, replace

**Minimal example:**

```jsx
import { useNavigate } from "react-router-dom"

export const IssueAdd = () => {
  const navigate = useNavigate()

  const createIssue = () => {
    // ... save issue ...
    navigate("/issues")
  }

  return <button onClick={createIssue}>Add Issue</button>
}
```

---

## 6) Nested Routes

**Definition:** Routes rendered inside a shared layout using an outlet.

**Key points:**

- Layout includes header/sidebar common to pages.
- `<Outlet />` is where child route content appears.

**Important terms / keywords:**

- Nested routes, layout route, `Outlet`

**Minimal example:**

```jsx
import { Outlet } from "react-router-dom"

export const AppLayout = () => (
  <div className="grid-container">
    <header>
      <h1>Issue Tracker</h1>
    </header>
    <nav>Sidebar Navigation</nav>
    <main>
      <Outlet />
    </main>
  </div>
)

// <Route path="/" element={<AppLayout />}>
//   <Route path="issues" element={<IssueList />} />
// </Route>
```

---

## 7) Browser History

**Definition:** The stack of navigation entries maintained by the browser.

**Key points:**

- **Push** adds a new entry (Back button works).
- **Replace** overwrites current entry (Back button skips).
- Common use: replace on login/redirect.

**Important terms / keywords:**

- History stack, push, replace

**Minimal example:**

```jsx
const navigate = useNavigate()
// Standard navigation
navigate("/issues")
// Replace history
navigate("/login", { replace: true })
```

---

## 8) Forms

**Definition:** UI for collecting user input using controlled components.

**Key points:**

- Controlled components store input values in React state.
- `onChange` updates state; `onSubmit` handles form action.
- `e.preventDefault()` avoids page reload.

**Important terms / keywords:**

- Controlled component, state, `onChange`, `onSubmit`

**Minimal example:**

```jsx
import { useState } from "react"

export const IssueAdd = ({ createIssue }) => {
  const [title, setTitle] = useState("")

  const handleSubmit = (e) => {
    e.preventDefault()
    createIssue({ title, status: "New" })
    setTitle("")
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        placeholder="Issue Title"
      />
      <button type="submit">Add</button>
    </form>
  )
}
```

**Controlled vs Uncontrolled (table):**

| Feature          | Controlled         | Uncontrolled        |
| ---------------- | ------------------ | ------------------- |
| State management | React state        | DOM handles value   |
| Data flow        | `onChange` + state | Refs to read values |
| Validation       | Real-time          | Usually on submit   |
| Initial value    | `value` prop       | `defaultValue` prop |

**Common Mistake:**

- ⚠️ Forgetting `e.preventDefault()` on submit.

---

## 9) Filter Form

**Definition:** A form that updates query string to filter displayed data.

**Key points:**

- Updates URL with selected filter value.
- Uses `navigate()` to add query string.
- Keeps filter state bookmarkable/shareable.

**Important terms / keywords:**

- Filter, query string update, `navigate`

**Minimal example:**

```jsx
export const IssueFilter = () => {
  const navigate = useNavigate()
  const [status, setStatus] = useState("New")

  const applyFilter = () => {
    navigate(`/issues?status=${status}`)
  }

  return (
    <div>
      <select onChange={(e) => setStatus(e.target.value)}>
        <option value="New">New</option>
        <option value="Assigned">Assigned</option>
      </select>
      <button onClick={applyFilter}>Filter</button>
    </div>
  )
}
```

---

## 10) GET API

**Definition:** Fetching data from the backend to display in UI.

**Key points:**

- `fetch('/api/issues')` retrieves list.
- Parse JSON and store in state.
- Render list using `map`.

**Important terms / keywords:**

- GET request, `fetch`, `useEffect`, JSON

**Minimal example:**

```jsx
import { useState, useEffect } from "react"

export const IssueList = () => {
  const [issues, setIssues] = useState([])

  useEffect(() => {
    const fetchIssues = async () => {
      const response = await fetch("/api/issues")
      const data = await response.json()
      setIssues(data.records)
    }
    fetchIssues()
  }, [])

  return (
    <ul>
      {issues.map((i) => (
        <li key={i.id}>{i.title}</li>
      ))}
    </ul>
  )
}
```

---

## 11) Edit Page

**Definition:** Page that loads a single record and pre-fills a form for editing.

**Key points:**

- Read `id` from route params.
- Fetch record by `id` to pre-fill fields.
- Use `defaultValue` or state to show existing values.

**Important terms / keywords:**

- Edit page, pre-fill, `useParams`, `useEffect`

**Minimal example:**

```jsx
export const IssueEdit = () => {
  const { id } = useParams()
  const [issue, setIssue] = useState({})

  useEffect(() => {
    const loadData = async () => {
      const res = await fetch(`/api/issues/${id}`)
      const data = await res.json()
      setIssue(data.issue)
    }
    loadData()
  }, [id])

  return <input defaultValue={issue.title} />
}
```

---

## 12) UI Components

**Definition:** Reusable visual building blocks used to compose the UI.

**Key points:**

- Encapsulate structure and behavior (list, form, button).
- Improve reuse and consistent UI across pages.

**Important terms / keywords:**

- Component, reuse, composition

---

## 13) Update API

**Definition:** Backend operation to modify an existing resource.

**Key points:**

- Uses `id` to target resource.
- Sends updated fields in request body.
- UI should refresh local state after update.

**Important terms / keywords:**

- Update, resource, payload, state refresh

---

## 14) Delete API

**Definition:** Backend operation to remove a resource.

**Key points:**

- Send `DELETE` request to `/api/issues/:id`.
- On success, remove item from local state list.

**Important terms / keywords:**

- DELETE request, remove, state update

**Minimal example:**

```jsx
const deleteIssue = async (id) => {
  const response = await fetch(`/api/issues/${id}`, {
    method: "DELETE",
  })

  if (response.ok) {
    setIssues((prevIssues) => prevIssues.filter((issue) => issue.id !== id))
  }
}
```

---

### Quick Revision Checklist

- ⭐ Know URL patterns for route params vs query strings.
- ⭐ Understand `useNavigate()` with push vs replace.
- ⭐ Use `<Outlet />` for nested routes.
- ⭐ Controlled form flow: state → input → submit.
- ⭐ CRUD flow: GET list, GET single for edit, UPDATE, DELETE.
