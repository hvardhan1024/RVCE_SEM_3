# Unit V – Working with React Router and Forms

> **Scope:** Routing Techniques, Simple Routing, Route Parameters, Route Query String, Programmatic Routing, Nested Routes, Browser History, Forms, Filter Form, GET API, Edit Page, UI Components, Update API, Delete API.

---

## 1) Routing Techniques

**Definition:** Techniques used by SPAs to change views by updating the URL without full page reloads.

**Key points:**

- ⭐ Client-side routing swaps components while keeping the page loaded.
- URL changes are handled by routing library (e.g., `react-router-dom`).
- Benefits: fast navigation, smooth UX, no full reload.
- **Hash-based routing** uses `#` portion; server never receives it.
- **Browser History (HTML5 History API)** uses `pushState`/`replaceState` to change URL without reload.
- ⚠️ History API routes require server support to serve the SPA for all routes.

**Important terms / keywords:**

- Client-side routing, Hash routing, History API, `pushState`, `replaceState`

**Example (URL patterns):**

- Hash: `https://example.com/#/products/42`
- History: `https://example.com/products/42`

---

## 2) Simple Routing

**Definition:** Defining main routes that map URL paths to components.

**Key points:**

- `<Routes>` groups route definitions.
- `<Route path="/" element={...} />` maps a path to a component.
- `path="*"` handles invalid routes (404).

**Important terms / keywords:**

- Routes, Route, path, element, wildcard route

**Example (minimal):**

- `/` → list page
- `/issues` → list page
- `*` → not found page

---

## 3) Route Parameters

**Definition:** Dynamic path segments used to identify specific resources.

**Key points:**

- `:id` declares a route parameter.
- `useParams()` reads parameters from URL.
- Used for single resource views like edit/detail pages.

**Important terms / keywords:**

- Route params, dynamic segments, `useParams`

**Example (URL):**

- `/issues/12345` where `id = 12345`

---

## 4) Route Query String

**Definition:** Optional key-value data appended to the URL for filtering or searching.

**Key points:**

- Format: `?key=value` pairs.
- Read using `useSearchParams()`.
- Typically used for filters (status, sort, search).

**Important terms / keywords:**

- Query string, query params, `useSearchParams`

**Example (URL):**

- `/issues?status=Open`

**Route Params vs Query Params (Quick Contrast):**

- **Route Params:** Mandatory, identify a single resource (`/issues/123`).
- **Query Params:** Optional, modify view (`/issues?status=Open`).

---

## 5) Programmatic Routing

**Definition:** Navigation triggered by code after an event (e.g., submit).

**Key points:**

- `useNavigate()` provides `navigate()`.
- ⭐ `navigate('/path')` **adds** a history entry.
- ⭐ `navigate('/path', { replace: true })` **replaces** current entry.

**Important terms / keywords:**

- Programmatic navigation, `useNavigate`, replace

**Example (use case):**

- Redirect to list page after creating an issue.

---

## 6) Nested Routes

**Definition:** Routes rendered inside a shared layout using an outlet.

**Key points:**

- Shared layout contains header/sidebar.
- `<Outlet />` is the placeholder for child routes.

**Important terms / keywords:**

- Nested routes, layout route, `Outlet`

**Example (concept):**

- Parent: `/` → layout
- Child: `/issues` → renders inside layout

---

## 7) Browser History

**Definition:** The stack of navigation entries maintained by the browser.

**Key points:**

- **Push** adds a new entry (Back button works).
- **Replace** overwrites current entry (Back button skips).
- Common use: replace on login/redirect.

**Important terms / keywords:**

- History stack, push, replace

---

## 8) Forms

**Definition:** UI for collecting user input, typically handled using controlled components.

**Key points:**

- Controlled components: input value stored in React state.
- `onChange` updates state; `onSubmit` handles form action.
- Prevent default form submit to avoid page reload.

**Important terms / keywords:**

- Controlled component, state, `onChange`, `onSubmit`

**Common Mistake:**

- ⚠️ Forgetting `e.preventDefault()` on submit.

---

## 9) Filter Form

**Definition:** A form that updates query string to filter displayed data.

**Key points:**

- Updates URL with selected filter (e.g., status).
- Can use `navigate()` to change query string.
- Keeps filter state bookmarkable/shareable.

**Important terms / keywords:**

- Filter, query string update, `navigate`

---

## 10) GET API

**Definition:** Fetching data from the backend to display in UI.

**Key points:**

- `fetch('/api/issues')` to get list.
- Parse JSON and store in state.
- Render list using map.

**Important terms / keywords:**

- GET request, `fetch`, `useEffect`, JSON

---

## 11) Edit Page

**Definition:** Page that loads a single record and pre-fills a form for editing.

**Key points:**

- Read `id` from route params.
- Fetch record by `id`.
- Bind fields to show existing values.

**Important terms / keywords:**

- Edit page, pre-fill, `useParams`, `useEffect`

---

## 12) UI Components

**Definition:** Reusable visual building blocks used to compose the UI.

**Key points:**

- Encapsulate structure and behavior (e.g., list, form, button).
- Promote reuse and consistency across pages.

**Important terms / keywords:**

- Component, reuse, composition

---

## 13) Update API

**Definition:** Backend operation to modify an existing resource.

**Key points:**

- Uses `id` to target the resource.
- Sends updated fields to the server.
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

---

### Quick Revision Checklist

- ⭐ Know URL patterns for route params vs query strings.
- ⭐ Understand `useNavigate()` with push vs replace.
- ⭐ Use `<Outlet />` for nested routes.
- ⭐ Controlled form flow: state → input → submit.
- ⭐ CRUD flow: GET list, GET single for edit, UPDATE, DELETE.
