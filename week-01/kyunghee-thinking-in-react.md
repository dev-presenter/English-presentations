# Thinking in React

Today we'll be discussing the article "Thinking in React" from the React documentation. This article is designed to help developers learn how to approach building React components and applications using a step-by-step thought process. When I first studied React, I felt that React had a high learning curve. The most difficult thing was that it was hard to know which task to start with. I've been reading the react documentation lately and thought it did a great job of explaining how to start working with react, so I thought I'd share it. We already know how to use React, but it's good to know what a react mindset is. Let's walk through the thought process of building a searchable product data table with React.

# Start with the mockup

- Imagine that you already have a JSON API and a mockup from a designer.

The JSON API returns some data that looks like this:

```json
[
  { "category": "Fruits", "price": "$1", "stocked": true, "name": "Apple" },
  { "category": "Fruits", "price": "$1", "stocked": true, "name": "Dragonfruit" },
  { "category": "Fruits", "price": "$2", "stocked": false, "name": "Passionfruit" },
  { "category": "Vegetables", "price": "$2", "stocked": true, "name": "Spinach" },
  { "category": "Vegetables", "price": "$4", "stocked": false, "name": "Pumpkin" },
  { "category": "Vegetables", "price": "$1", "stocked": true, "name": "Peas" }
]
```

The mockup looks like this:

![https://beta.reactjs.org/images/docs/s_thinking-in-react_ui.png](https://beta.reactjs.org/images/docs/s_thinking-in-react_ui.png)

To implement a UI in React, you will usually follow the same five steps.

# Step 1: Break the UI into a component hierarchy

- Start by drawing boxes around every component and subcomponent in the mockup and naming them. Separate your UI into components, where each component matches one piece of your data model.

There are five components on this screen:

![https://beta.reactjs.org/images/docs/s_thinking-in-react_ui_outline.png](https://beta.reactjs.org/images/docs/s_thinking-in-react_ui_outline.png)

1. `FilterableProductTable` (grey) contains the entire app.
2. `SearchBar` (blue) receives the user input.
3. `ProductTable` (lavender) displays and filters the list according to the user input.
4. `ProductCategoryRow` (green) displays a heading for each category.
5. `ProductRow` (yellow) displays a row for each product.

- Now that you’ve identified the components in the mockup, arrange them into a hierarchy. Components that appear within another component in the mockup should appear as a child in the hierarchy:

```
- FilterableProductTable
| - SearchBar
| - ProductTable
  | - ProductCategoryRow
  | - ProductRow
```

# Step 2: Build a static version in React

- It's time to build a version that renders the UI from your data model. This version should **not** include any interactivity or state management.
- If you’re familiar with the concept of state, don’t use state at all to build this static version. State is reserved only for interactivity, that is, data that changes over time. Since this is a static version of the app, you don’t need it.
- You can either build “top down” by starting with building the components higher up in the hierarchy (like `FilterableProductTable`) or “bottom up” by working from components lower down (like `ProductRow`). In simpler examples, it’s usually easier to go top-down, and on larger projects, it’s easier to go bottom-up.

```jsx
function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  ...
  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products }) {
	...
  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar() {
	...
  return (
    <form>
      <input type="text" placeholder="Search..." />
      <label>
        <input type="checkbox" />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

function FilterableProductTable({ products }) {
	...
  return (
    <div>
      <SearchBar />
      <ProductTable products={products} />
    </div>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  ...
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

# Step 3: Find the minimal but complete representation of UI state

- To make the UI interactive, you need to let users change your underlying data model. You will use _state_ for this.
- **State**: the minimal set of **changing data** that your app needs to remember
- The most important principle for structuring state is to keep it **DRY (Don’t Repeat Yourself)**.
- It’s **not a state** if any of the following three conditions are true:
  - It does **not change over time**
  - it’s **passed in from a parent** via props
  - You can **compute it** based on existing state or props in your component
- Now think of all of the pieces of data in this example application:
  1. The original list of products
     → **Not state** / It is passed in as props.
  2. The search text the user has entered
     → **State** / It seems to be state since it changes over time and can’t be computed from anything.
  3. The value of the checkbox
     → **State** / It seems to be state since it changes over time and can’t be computed from anything.
  4. The filtered list of products
     → **Not state** / \*\*\*\*It can be computed by taking the original list of products and filtering it according to the search text and value of the checkbox.

# Step 4: Identify where your state should live

- After identifying your app’s minimal state data, you need to identify which component is responsible for changing this state, or _owns_ the state.
- Remember: React uses one-way data flow, passing data down the component hierarchy from parent to child component. It may not be immediately clear which component should own what state.
- Now let’s run through our strategy for them:
  1. **Identify components that use state:**
     - `ProductTable` needs to filter the product list based on that state (search text and checkbox value).
     - `SearchBar` needs to display that state (search text and checkbox value).
  2. **Find their common parent:** The first parent component both components share is `FilterableProductTable`.
  3. **Decide where the state lives**: We’ll keep the filter text and checked state values in `FilterableProductTable`.
     ⇒ So the state values will live in `FilterableProductTable`.
- Add state to the component with the `useState()` Hook. Hooks are special functions that let you “hook into” React.
- However, you will encounter the following error in the console. Because you haven’t added any code to respond to the user actions.
  > You provided a `value` prop to a form field without an `onChange` handler. This will render a read-only field.

```jsx
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('')
  const [inStockOnly, setInStockOnly] = useState(false)

  return (
    <div>
      <SearchBar filterText={filterText} inStockOnly={inStockOnly} />
      <ProductTable products={products} filterText={filterText} inStockOnly={inStockOnly} />
    </div>
  )
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = []
  let lastCategory = null

  products.forEach((product) => {
    if (product.name.toLowerCase().indexOf(filterText.toLowerCase()) === -1) {
      return
    }
    if (inStockOnly && !product.stocked) {
      return
    }
    if (product.category !== lastCategory) {
      rows.push(<ProductCategoryRow category={product.category} key={product.category} />)
    }
    rows.push(<ProductRow product={product} key={product.name} />)
    lastCategory = product.category
  })

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  )
}

function SearchBar({ filterText, inStockOnly }) {
  return (
    <form>
      <input type="text" value={filterText} placeholder="Search..." />
      <label>
        <input type="checkbox" checked={inStockOnly} /> Only show products in stock
      </label>
    </form>
  )
}
```

# Step 5: Add inverse data flow

You want to make it so whenever the user changes the form inputs, the state updates to reflect those changes. The state is owned by `FilterableProductTable`, so only it can call `setFilterText` and `setInStockOnly`. To let `SearchBar` update the `FilterableProductTable`’s state, you need to pass these functions down to `SearchBar`. And inside the `SearchBar`, you will add the `onChange` event handlers and set the parent state from them:

```jsx
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('')
  const [inStockOnly, setInStockOnly] = useState(false)

  return (
    <div>
      <SearchBar
        filterText={filterText}
        inStockOnly={inStockOnly}
        onFilterTextChange={setFilterText}
        onInStockOnlyChange={setInStockOnly}
      />
      <ProductTable products={products} filterText={filterText} inStockOnly={inStockOnly} />
    </div>
  )
}

function SearchBar({ filterText, inStockOnly }) {
  return (
    <form>
      <input
        type="text"
        value={filterText}
        placeholder="Search..."
        onChange={(e) => onFilterTextChange(e.target.value)}
      />
      <label>
        <input type="checkbox" checked={inStockOnly} /> Only show products in stock
      </label>
    </form>
  )
}
```

So that was a brief summary of the article "Thinking in React" from the React documentation. By following the step-by-step thought process outlined in the article, you can learn how to approach building React components and applications in a more efficient and manageable way. Thank you for listening!

---

# Reference

[React Docs](https://react.dev/learn/thinking-in-react). Meta Open Source. (2023).
