# Notes

---

| Framework / Stack         | Compiler / Transpiler      | Bundler                         |
| ------------------------- | -------------------------- | ------------------------------- |
| **React (vanilla)**       | Babel                      | Webpack                         |
| **Next.js (Page Router)** | Babel                      | Webpack                         |
| **Next.js (App Router)**  | SWC                        | Turbopack (or Webpack fallback) |
| **Vite + React**          | esbuild (dev), Babel (opt) | Vite (uses Rollup in prod)      |


---


| Tool          | Role(s)                                                    | Used In                             | Written In |
| ------------- | ---------------------------------------------------------- | ----------------------------------- | ---------- |
| **Babel**     | Compiler / Transpiler                                      | React, legacy Next.js (Page Router) | JavaScript |
| **SWC**       | Compiler / Transpiler                                      | Next.js (App Router)                | Rust       |
| **esbuild**   | Compiler / Transpiler + Bundler                            | Vite, other modern tools            | Go         |
| **Webpack**   | Bundler                                                    | React, legacy Next.js               | JavaScript |
| **Turbopack** | Bundler                                                    | Next.js (App Router, experimental)  | Rust       |
| **Vite**      | Dev Server + Bundler (uses esbuild in dev, Rollup in prod) | Vite + React                        | TypeScript |



---

### 1. **What is Virtual DOM?**

> Virtual DOM (V-DOM) is a lightweight in-memory representation of the Real DOM.
> React uses it to efficiently calculate UI changes by comparing previous and current V-DOM (using the diffing algorithm), then updates only the changed parts on the Real DOM.

---

### 2. **What is the difference between Real DOM and Virtual DOM?**

| Real DOM             | Virtual DOM         |
| -------------------- | ------------------- |
| Actual UI in browser | In-memory JS object |
| Expensive to update  | Cheap to update     |
| Slow manipulation    | Fast calculation    |
| Rendered by browser  | Managed by React    |

---

### 3. **What is BOM (Browser Object Model)?**

> BOM represents browser-specific objects like `window`, `navigator`, `localStorage`, etc.
> Unlike DOM, BOM has no standard specification (browser-dependent).

---

### 4. **What is the Document Object Model (DOM)?**

> DOM represents the structured HTML elements of a webpage as a tree of objects (nodes).
> JavaScript can manipulate the DOM using methods like `document.getElementById()`.

---

### 5. **Explain React Reconciliation Algorithm**]]]

> Reconciliation is React's process of comparing the old Virtual DOM tree with the new Virtual DOM tree, then calculating the minimal number of changes needed to update the Real DOM.

---

### 6. **What is React Diffing Algorithm?**

> It’s the algorithm React uses **during reconciliation** to compare old vs new V-DOM trees.
> It uses heuristics like:

* Comparing sibling elements by index
* Reusing nodes with the same key
* Minimizing unnecessary DOM operations

---

### 7. **What is the internal flow? (Diffing → Reconciliation)**]]]

✅ Simple flow during state update:

```
setState → Re-render → New Virtual DOM → Diffing → Reconciliation → Real DOM Updates
```

---

### 8. **Controlled vs Uncontrolled Components**

|             | Controlled                 | Uncontrolled                  |
| ----------- | -------------------------- | ----------------------------- |
| Data Source | React State                | DOM                           |
| Example     | `useState`, `onChange`     | `ref`, uncontrolled input     |
| Good for    | Complex forms, validations | Simple forms, low computation |
| Behavior    | Fully React controlled     | Browser-controlled            |

---

### 9. **What is React Fiber?**]]]

> Fiber is React's **new reconciliation engine (from React 16)**.
> It allows React to split rendering work into small units and prioritize urgent updates (like user inputs), making rendering **interruptible and non-blocking**.

---

### 10. **React Hooks Lifecycle & Internal Flow**]]]

| Hook                 | When it runs                                    |
| -------------------- | ----------------------------------------------- |
| `useEffect`          | After paint (async)                             |
| `useLayoutEffect`    | After DOM mutation but before paint             |
| `useInsertionEffect` | Before DOM insertion, useful for CSS-in-JS libs |

✅ Hooks follow this order:

```
Mount → Update → Unmount
```

---

### 11. **What happens internally when useState is called?**]]]

> React keeps state values **inside a Fiber Node's hook list**.
> On every render:

* React calls your component again
* Hooks are re-executed in same order
* React uses internal state slots to remember state values
* State updates trigger re-render + reconciliation

---

### 12. **What is Context API in React?**

> Context API provides a way to share state or values **globally across deeply nested components**, **without prop drilling**.

✅ Steps:

* Create Context
* Provide context at top level
* Consume context anywhere in tree using `useContext`

---

### 13. **Difference: useEffect vs useLayoutEffect vs useInsertionEffect**]]]

| Hook                 | Runs When            | Use Case                                    |
| -------------------- | -------------------- | ------------------------------------------- |
| `useEffect`          | After paint          | Data fetching, side effects                 |
| `useLayoutEffect`    | Before paint         | Measuring layout, synchronizing DOM changes |
| `useInsertionEffect` | Before DOM insertion | Inject styles (CSS-in-JS libs like Emotion) |

---

### 14. **What is React.memo?**]]]

> `React.memo` is a **higher-order component** that **caches** the rendered output of a component.
> It **prevents unnecessary re-renders** when props haven’t changed.

✅ Good for **performance optimization of pure components**.

---

### 15. **Difference: useCallback vs useMemo**

| Hook          | Returns           | Use Case                                |
| ------------- | ----------------- | --------------------------------------- |
| `useCallback` | Memoized function | Prevent re-creating functions           |
| `useMemo`     | Memoized value    | Prevent re-calculating expensive values |

✅ Both useful in **performance-heavy components**.

---

### 16. **What is React Event Delegation Model?**]]]

> React **attaches one global event listener per event type on the document (using Synthetic Events)**.
> All events bubble up through this listener, **instead of attaching separate listeners to each DOM node**.

✅ Benefits: Performance, consistency across browsers.

---

### 17. **What are Higher-Order Components (HOCs)?**]]

> An **HOC** is a function that **takes a component and returns a new enhanced component**.

Example:

```ts
const withLogger = (Component) => {
  return (props) => {
    console.log('Props:', props);
    return <Component {...props} />;
  };
};
```

---

### 18. **What is Higher-Order Function (HOF)?**

> A **HOF** is a plain JavaScript function that:

* Takes one or more functions as arguments **or**
* Returns a function

Examples: `map`, `filter`, `reduce`

---

### 19. **What are Render Props?**]]]

> Render Props is a pattern where **you pass a function as a prop**, and that function returns React elements.

Example:

```tsx
<MouseTracker render={(position) => <h1>Mouse at {position.x}, {position.y}</h1>} />
```

---

### 20. **What are Compound Components in React?**]]]

> **Compound Components** are components designed to **work together** and **share implicit state**.

✅ Example:
A `<Tabs>` component with nested `<Tab />` components that **all communicate with each other internally** using React Context.

Example usage:

```tsx
<Tabs>
  <Tab>Tab 1</Tab>
  <Tab>Tab 2</Tab>
</Tabs>
```


### ✅ 21. What is a Custom Hook and when would you write one?

**Custom Hook = A reusable JavaScript function that starts with `use` and uses one or more React Hooks inside it.**

---

| Why write one?                                | Example Use Cases                                                |
| --------------------------------------------- | ---------------------------------------------------------------- |
| To **reuse logic across multiple components** | Data fetching, form state, debouncing                            |
| To **abstract complex logic**                 | Common API calls (like `useHeaderQuery`, as you use in TechNova) |
| To **reduce code duplication**                | Pagination logic, user authentication checks                     |

**Example:**

```ts
function useFetchData<T>(url: string): T {
  const [data, setData] = useState<T | null>(null);
  useEffect(() => {
    fetch(url).then((res) => res.json()).then(setData);
  }, [url]);
  return data;
}
```

---

### ✅ 22. Explain the concept of Error Boundaries.]]]

**Error Boundaries = React components that catch JavaScript errors in their child component tree, log them, and render fallback UI without crashing the whole app.**

| Key Facts         | Details                                                     |
| ----------------- | ----------------------------------------------------------- |
| Implemented using | Class Components only (as of now)                           |
| Catchable errors  | During rendering, lifecycle methods, and constructors       |
| Cannot catch      | Async errors like event handlers, setTimeout, server errors |
| Example           | Network error fallback, rendering fallback component        |

**Example:**

```ts
class ErrorBoundary extends React.Component {
  state = { hasError: false };
  static getDerivedStateFromError() { return { hasError: true }; }
  componentDidCatch(error, info) { console.error(error, info); }
  render() { return this.state.hasError ? <Fallback /> : this.props.children; }
}
```

---

### ✅ 23. Difference between React.forwardRef and useRef?]]]

|             | `React.forwardRef`                           | `useRef`                                  |
| ----------- | -------------------------------------------- | ----------------------------------------- |
| Purpose     | Pass a `ref` from parent to child            | Create a ref inside a component           |
| When to use | When a parent wants to control a child’s DOM | When a component needs internal reference |
| Use case    | Exposing child input DOM element to parent   | Storing mutable values or DOM elements    |
| Example     | Custom Input component forwarding ref        | Scroll position, input element reference  |

**Example using forwardRef:**

```ts
const MyInput = React.forwardRef((props, ref) => (
  <input ref={ref} {...props} />
));
```

---

### ✅ 24. How does React Suspense work?

**React Suspense = Mechanism for showing fallback UI (like loaders) while waiting for something like:**

* Code Splitting
* Server Components
* Data Fetching (with frameworks like Relay or Next.js RSC)

| Key Points   |                                                                |
| ------------ | -------------------------------------------------------------- |
| What it does | Allows you to pause rendering until data or component is ready |
| Used for     | Lazy loading components, waiting for server components         |
| How          | By wrapping with `<Suspense fallback={<Loader />}>`            |

**Example:**

```tsx
<Suspense fallback={<div>Loading...</div>}>
  <MyLazyComponent />
</Suspense>
```

---

### ✅ 25. What is lazy loading in React and how to implement it?

**Lazy Loading = Load heavy or non-critical components only when needed (instead of at initial load).**

| Why? | Improve performance, reduce initial bundle size |
| ---- | ----------------------------------------------- |
| How? | Using `React.lazy()` and `<Suspense>`           |

**Example:**

```tsx
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));

<Suspense fallback={<div>Loading...</div>}>
  <HeavyComponent />
</Suspense>
```

✅ Next.js does this automatically for pages/routes using dynamic imports.

---

### ✅ 26. Explain React Profiler API and when to use it.]]]

**React Profiler API = Tool to measure performance (re-render time, frequency, etc.)**

| How?       | Wrap components with `<Profiler>` in dev builds                         |
| ---------- | ----------------------------------------------------------------------- |
| Metrics    | Actual render time, base render time, render reasons                    |
| Where used | Performance bottleneck analysis, production testing with React DevTools |

**Example:**

```tsx
<Profiler id="MyComponent" onRender={(id, phase, actualTime) => {
  console.log(`${id} rendered in ${actualTime}ms`);
}}>
  <MyComponent />
</Profiler>
```

✅ Also available in **React DevTools** Profiler Tab.

---

### ✅ 27. How do you handle Prop Drilling at scale?

**Prop Drilling = Passing data down multiple nested component levels.**

| Problems                        | Solutions                            |
| ------------------------------- | ------------------------------------ |
| Becomes messy, hard to maintain | ✅ Context API                        |
| Causes unnecessary renders      | ✅ Zustand / Redux (for global state) |
| Deeply nested components        | ✅ Component composition / slots      |
| Over-rendering                  | ✅ Memoization (React.memo, useMemo)  |

✅ Best practice for large Next.js apps:
👉 Use **Context API for small scale**
👉 **Zustand / Redux Toolkit / React Query for larger, global state**



## ✅ 28. Explain the difference between SSR, SSG, ISR, and Client-Side Rendering in Next.js.

|                           | SSR (Server-Side Rendering)          | SSG (Static Site Generation) | ISR (Incremental Static Regeneration)                        | CSR (Client-Side Rendering)                      |
| ------------------------- | ------------------------------------ | ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------ |
| When content is generated | On every request                     | At build time                | Pre-generated, but can regenerate in background after deploy | In browser after page load                       |
| Use case                  | Dynamic pages (user data, auth, etc) | Fully static pages           | Mix of static + dynamic (like blog with updates)             | Highly dynamic UI / Single Page Apps             |
| Next.js methods           | `getServerSideProps()`               | `getStaticProps()`           | `getStaticProps` + `revalidate`                              | Data fetching inside `useEffect`, or React Query |
| SEO friendly?             | ✅ Yes                                | ✅ Yes                        | ✅ Yes                                                        | ❌ Not SEO friendly                               |

---

## ✅ 29. What are Server Components (RSC) in Next.js App Router?

**Server Components (RSC) = React components that run entirely on the server and never get included in the browser JavaScript bundle.**

| Feature         | Details                                                           |
| --------------- | ----------------------------------------------------------------- |
| Runs            | Only on server                                                    |
| Can access      | Server-side resources (DB, filesystem, secrets)                   |
| Sends to client | Only the **render result (RSC payload)**, not actual component JS |
| Benefit         | Reduces client-side bundle size                                   |

✅ In Next.js App Router:
Server Components = Default.
Client Components = Need `"use client"`.

---

## ✅ 30. Explain Server Actions in Next.js.]]]

**Server Actions = Next.js feature allowing you to run server-side functions directly from React components (like form submissions or mutations), without writing API routes.**

| Feature        | Details                                                    |
| -------------- | ---------------------------------------------------------- |
| Runs           | On server only                                             |
| Triggered by   | Form submission or programmatically via function call      |
| Example use    | Saving to database, sending emails, etc                    |
| How to declare | Mark function with `'use server'`                          |
| Benefit        | Avoid manual API endpoint creation for simple server logic |

**Example:**

```ts
'use server';
async function saveUser(formData: FormData) {
  // Server-side logic
}
```

---

## ✅ 31. What is the RSC payload and how does it work in streaming?]]]

**RSC Payload = Serialized data (JSON-like or binary stream) sent from Next.js server to the browser describing rendered Server Components.**

| Feature      | Details                                                                                             |
| ------------ | --------------------------------------------------------------------------------------------------- |
| Sent during  | Page load, client navigation, server actions                                                        |
| Contains     | Component tree, props, HTML instructions                                                            |
| Sent as      | Streamed chunks over HTTP                                                                           |
| Processed by | React client runtime (to hydrate UI)                                                                |
| Streaming    | Enables **progressive rendering** (send parts of UI as soon as ready, not wait for entire page)\*\* |

You’ll see it in **Network tab → `.rsc` or `_rsc` requests**.

---

## ✅ 32. What happens during Next.js hydration?]]]

**Hydration = Process where React takes over server-rendered HTML and attaches browser event listeners + client-side logic to make it interactive.**

| Step | What happens                                     |
| ---- | ------------------------------------------------ |
| 1    | HTML comes from server (SSR/SSG)                 |
| 2    | Browser loads React JS bundle                    |
| 3    | React reconciles existing HTML with Virtual DOM  |
| 4    | Adds interactivity (event listeners, state, etc) |
| 5    | UI becomes interactive                           |

✅ For **Client Components**, hydration is mandatory.]]]
✅ For **Server Components**, only client-interactive parts hydrate.

---

## ✅ 33. How does Next.js handle routing in App Directory vs Pages Directory?]]]

|                   | App Directory (App Router)                     | Pages Directory (Pages Router)                            |
| ----------------- | ---------------------------------------------- | --------------------------------------------------------- |
| Routing system    | File-based + nested layouts                    | File-based, flat structure                                |
| Supports RSC      | ✅ Yes                                          | ❌ No                                                      |
| Data fetching     | Server Components + `fetch()` + Server Actions | `getServerSideProps`, `getStaticProps`, `getInitialProps` |
| Layouts           | Nested layouts                                 | No layouts                                                |
| Default rendering | Server Components                              | Client Components                                         |
| Used from         | Next.js 13+                                    | Next.js legacy                                            |

---

## ✅ 34. How do you implement API routes in Next.js?

|           |                                                 |
| --------- | ----------------------------------------------- |
| Directory | Inside `/pages/api/` folder (Pages Router only) |
| Exports   | Default function handler `(req, res)`           |
| Runs      | Only on server                                  |
| Example   | `/pages/api/user.ts` → `/api/user`              |

**Example:**

```ts
export default function handler(req, res) {
  res.status(200).json({ name: 'John Doe' });
}
```

✅ For **App Router**, you should use **Route Handlers (`app/api/`)**.

---

## ✅ 35. Explain Dynamic Routing and Catch-All routes in Next.js.]]]

|                    | Dynamic Routes                | Catch-All Routes                     |
| ------------------ | ----------------------------- | ------------------------------------ |
| File Naming        | `[param].tsx`                 | `[...params].tsx`                    |
| URL Example        | `/products/123`               | `/docs/anything/deep/path`           |
| Purpose            | Handle single dynamic segment | Handle multiple dynamic segments     |
| Catch-All Optional | `[...slug]`                   | `[[...slug]]` for optional catch-all |

---

## ✅ 36. What is ISR (Incremental Static Regeneration) and how does Next.js implement it?]]]

**ISR = Allows Next.js to serve static pages with the ability to revalidate and rebuild them in the background after deployment.**

| Feature           | Details                                           |
| ----------------- | ------------------------------------------------- |
| Used with         | `getStaticProps` with `revalidate`                |
| When page updates | After timeout (or via on-demand revalidation API) |
| Why               | Mix benefits of SSG and SSR                       |
| Example           | Blogs, product pages                              |

**Example:**

```ts
export async function getStaticProps() {
  return {
    props: {},
    revalidate: 60,  // Rebuild page in background after 60 seconds
  };
}
```

---

## ✅ 37. Explain Middleware in Next.js and use cases.

**Middleware = Runs before a request is completed (edge function).
Can modify request, redirect, rewrite or add headers before sending response.**

| Feature   | Details                                                  |
| --------- | -------------------------------------------------------- |
| Location  | `/middleware.ts`                                         |
| Runs      | At Edge (before request completes)                       |
| Can       | Redirect, rewrite, add headers, block unauthorized users |
| Use Cases | Auth redirects, feature flags, geolocation-based routing |

**Example:**

```ts
export function middleware(request: NextRequest) {
  const userToken = request.cookies.get('token');
  if (!userToken) {
    return NextResponse.redirect('/login');
  }
}
```

---


✅ Here's a full **senior-level answer guide for Questions 38-50**, covering **State Management**, **React Query**, **TypeScript**, **Performance**, and **Testing** for React / Next.js:

---

## 🔷 State Management & Data Fetching:

---

### ✅ 38. Difference between Redux, Zustand, React Context, and React Query:

|                | Redux                          | Zustand                        | React Context          | React Query                        |
| -------------- | ------------------------------ | ------------------------------ | ---------------------- | ---------------------------------- |
| Purpose        | Centralized state management   | Minimal, flexible global state | Passing global props   | Server state management (API data) |
| Boilerplate    | High                           | Low                            | Low                    | Medium                             |
| Async handling | Needs middleware (Thunk, Saga) | Built-in                       | Not designed for async | Built-in                           |
| Best for       | Large app-wide state           | Small to medium app state      | Simple app-wide values | Data fetching, caching             |

---

### ✅ 39. What is React Query? How does caching and stale-time work?

**React Query = Data-fetching & caching library for React apps**

| Concept            | Description                                         |
| ------------------ | --------------------------------------------------- |
| Caching            | Stores API responses in memory, prevents refetching |
| Stale Time         | Duration for which fetched data is considered fresh |
| Refetching         | After staleTime ends, auto-refetches                |
| Background Updates | Keeps UI fast while data stays fresh                |

**Example:**

```ts
useQuery('users', fetchUsers, { staleTime: 5 * 60 * 1000 });
```

---

### ✅ 40. How does React Query handle mutations and optimistic updates?

|                    |                                                  |
| ------------------ | ------------------------------------------------ |
| Mutation           | For POST/PUT/DELETE actions                      |
| Optimistic Update  | Temporarily updates UI before server confirms    |
| Rollback           | Reverts if API fails                             |
| Cache Invalidation | Manually refetch relevant queries after mutation |

**Example:**

```ts
const mutation = useMutation(updateUser, {
  onMutate: async (newData) => {
    await queryClient.cancelQueries('user');
    const previousData = queryClient.getQueryData('user');
    queryClient.setQueryData('user', newData);  // Optimistic update
    return { previousData };
  },
  onError: (err, newData, context) => {
    queryClient.setQueryData('user', context.previousData);  // Rollback
  },
  onSettled: () => {
    queryClient.invalidateQueries('user');
  }
});
```

---

### ✅ 41. How to handle global state in large React apps?

| Strategy                | When to use                                      |
| ----------------------- | ------------------------------------------------ |
| Context API             | Small-scale app-wide state                       |
| Redux Toolkit / Zustand | Medium to large scale local state                |
| React Query / SWR       | Server-side, API data                            |
| Apollo Client           | If working with GraphQL                          |
| Best practice           | Split server state vs client UI state management |

---

### ✅ 42. What is SWR and how does it differ from React Query?

|                  | SWR                             | React Query                               |
| ---------------- | ------------------------------- | ----------------------------------------- |
| Full Form        | Stale-While-Revalidate          | React Query                               |
| Maintainer       | Vercel                          | TanStack                                  |
| Focus            | Simplicity, mainly GET requests | Full control: queries, mutations, caching |
| API design       | Minimalist                      | More config options                       |
| Mutation support | Limited                         | Advanced                                  |

---

## 🔷 TypeScript in React:

---

### ✅ 43. When do you use type vs interface in TypeScript?

|                     | Type                       | Interface                |
| ------------------- | -------------------------- | ------------------------ |
| Extending           | Not extendable             | Can extend               |
| Declaration merging | ❌ No                       | ✅ Yes                    |
| Best for            | Union types, complex types | Props, object structures |

**✅ Rule of thumb:**
Use **`interface` for components & props**, **`type` for unions & utilities**.

---

### ✅ 44. Explain Generics in React Components with examples.

**Generics = Writing reusable code with type flexibility.**

**Example:**

```ts
type ListProps<T> = { items: T[]; renderItem: (item: T) => React.ReactNode; };

function List<T>({ items, renderItem }: ListProps<T>) {
  return <div>{items.map(renderItem)}</div>;
}
```

✅ Now `List` works with **array of users**, **array of products**, etc.

---

### ✅ 45. How do you type API response and component props in Next.js?

|                 |                                          |
| --------------- | ---------------------------------------- |
| API Response    | Use TypeScript interfaces or Zod schemas |
| Component Props | Use interfaces or types                  |
| React Query     | `useQuery<T>()` generic                  |
| Example:        |                                          |

```ts
type User = { id: number; name: string; };
const { data } = useQuery<User[]>('users', fetchUsers);
```

---

### ✅ 46. How would you create a strongly typed custom hook in React?

**Example:**

```ts
function useFetchUser<T>(url: string): T | undefined {
  const [data, setData] = useState<T>();
  useEffect(() => {
    fetch(url).then((res) => res.json()).then(setData);
  }, [url]);
  return data;
}
```

✅ Benefit:
Strong typing for API responses across app.

---

### ✅ 47. How do you handle form type safety with react-hook-form and Zod?

| Step | What to do               |
| ---- | ------------------------ |
| 1    | Create Zod schema        |
| 2    | Use `zodResolver`        |
| 3    | Use `useForm<ZodType>()` |

**Example:**

```ts
const schema = z.object({ name: z.string(), age: z.number() });

const { register, handleSubmit } = useForm<z.infer<typeof schema>>({
  resolver: zodResolver(schema),
});
```

✅ Prevents runtime form data errors.

---

## 🔷 Performance Optimization:

---

### ✅ 48. How to prevent unnecessary re-renders in React?

| Technique               | Purpose                           |
| ----------------------- | --------------------------------- |
| React.memo              | Prevents child re-render          |
| useCallback             | Prevents function recreation      |
| useMemo                 | Prevents expensive recalculations |
| key prop                | Helps React track lists           |
| Split large components  | Lazy load sub-components          |
| React DevTools Profiler | Identify slow renders             |

---

### ✅ 49. Difference between useMemo and useCallback?

|          | useMemo                        | useCallback                    |
| -------- | ------------------------------ | ------------------------------ |
| Returns  | Memoized value                 | Memoized function              |
| Use case | Expensive calculations         | Stable function reference      |
| Syntax   | `const result = useMemo(...);` | `const fn = useCallback(...);` |

---

### ✅ 50. What is React Concurrent Mode?

> **Concurrent Mode = New React rendering behavior (behind flags or future React versions) where React can interrupt, pause, resume, and prioritize rendering work.**

✅ Enables:

* Better responsiveness
* Non-blocking rendering
* Features like **Suspense for Data Fetching**, **useTransition**, **Streaming UI**

*(As of 2025, still under gradual adoption)*

---

### ✅ 51. How do you code-split a large React app?

| Technique             |                                 |
| --------------------- | ------------------------------- |
| React.lazy            | For component level             |
| Dynamic imports       | In Next.js (`next/dynamic`)     |
| Bundle splitting      | Webpack / Turbopack built-in    |
| Route level splitting | Automatic in Next.js App Router |

---

### ✅ 52. How do you measure TTFB, LCP, FCP, and TTI in a Next.js app?

| Metric | Meaning                  | How to measure                                 |
| ------ | ------------------------ | ---------------------------------------------- |
| TTFB   | Time to First Byte       | Next.js built-in telemetry or Vercel analytics |
| LCP    | Largest Contentful Paint | Web Vitals package or Lighthouse               |
| FCP    | First Contentful Paint   | Web Vitals, Google Analytics                   |
| TTI    | Time to Interactive      | Lighthouse                                     |

✅ Next.js can auto-collect Web Vitals using `reportWebVitals()`.

---

## 🔷 Testing in React:

---

### ✅ 53. How do you test React components? Which libraries do you use?

|                  |                              |
| ---------------- | ---------------------------- |
| Unit testing     | Jest + React Testing Library |
| E2E testing      | Cypress / Playwright         |
| Test coverage    | Jest Coverage Report         |
| Snapshot testing | Jest snapshots               |

---

### ✅ 54. Difference between unit tests and integration tests in React?

| Unit Test                       | Integration Test                           |
| ------------------------------- | ------------------------------------------ |
| Tests single function/component | Tests multiple components working together |
| Mock dependencies               | Includes real component rendering          |
| Example: Button click handler   | Example: Form submitting data flow         |

---

### ✅ 55. How do you mock API calls during testing?

| Library                    | Example                         |
| -------------------------- | ------------------------------- |
| jest.mock                  | Mock fetch/Axios                |
| MSW (Mock Service Worker)  | Intercept real network requests |
| React Query test utilities | Mock API layer                  |

**Example:**

```ts
jest.mock('axios', () => ({ get: jest.fn() }));
```

---

### ✅ 56. Explain testing asynchronous hooks or components (like useQuery):

| Step | What to do                                                      |
| ---- | --------------------------------------------------------------- |
| 1    | Wrap component in QueryClientProvider                           |
| 2    | Use React Testing Library’s async helpers (`waitFor`, `findBy`) |
| 3    | Mock API layer                                                  |

**Example:**

```ts
await waitFor(() => expect(screen.getByText('User Loaded')).toBeInTheDocument());
```

---

### ✅ 57. How do you perform end-to-end testing for Next.js apps? (Example: Playwright / Cypress)

| Tool    | Playwright / Cypress                                |
| ------- | --------------------------------------------------- |
| Type    | Browser automation                                  |
| Test    | Full user flows: Login, Navigation, Form submission |
| How     | Run against deployed site or dev server             |
| Example | Click button → Fill form → Assert success message   |

---



