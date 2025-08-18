<div>
  <h1>useMemo</h1>
  <h2>What useMemo Does</h2>
  <p>
    <code>useMemo</code> lets you memoize (remember) a computed value so that it doesn't
    get recalculated on every render, unless its dependencies change.
  </p>
  <dl>
    <dt>This is useful when:</dt>
    <dd>
      <ul>
        <li>The calculation is expensive (takes time or resources).</li>
        <li>You want to avoid unnecessary re-computations when unrelated state changes.</li>
      </ul>
    </dd>
  </dl>

  <h2>Real-World Example: Filtering a Large List</h2>
  <pre><code class="language-js">
import React, { useState, useMemo } from "react";

export default function ProductFilter() {
  const [search, setSearch] = useState("");
  const products = ["Laptop", "Phone", "Tablet", "Camera", "Smartwatch"];

  const filteredProducts = useMemo(() => {
    console.log("Filtering...");
    return products.filter(item =>
      item.toLowerCase().includes(search.toLowerCase())
    );
  }, [search, products]);

  return (
    &lt;div&gt;
      &lt;input
        type="text"
        placeholder="Search products..."
        value={search}
        onChange={(e) =&gt; setSearch(e.target.value)}
      /&gt;
      &lt;ul&gt;
        {filteredProducts.map((p, index) =&gt; (
          &lt;li key={index}&gt;{p}&lt;/li&gt;
        ))}
      &lt;/ul&gt;
    &lt;/div&gt;
  );
}
  </code></pre>
</div>

<div>
  <h1>*******************************************************************************🎈 What useCallback Does ************************************************************</h1>
  <h1>useCallback</h1>
  <h2>What useCallback Does</h2>
  <p>
    <code>useCallback</code> lets you memoize a function definition so that it is not
    re-created on every render, unless its dependencies change.
  </p>
  <dl>
    <dt>This is useful when:</dt>
    <dd>
      <ul>
        <li>You pass a callback to a child component that relies on referential equality to prevent unnecessary re-renders.</li>
        <li>You want to avoid creating a new function on every render for performance optimization.</li>
      </ul>
    </dd>
  </dl>
<div>
  
<p>useCallback returns a memoized function that only changes when its dependencies change.
It’s used to prevent re-creation of functions on every render, which is helpful when:

Passing functions as props to child components (avoiding unnecessary re-renders).

Avoiding function identity change when dependencies didn’t change.
</p>
<h3>Basic Example</h3>
  <pre><code class="language-js">
import React, { useState, useCallback } from "react";

function Button({ onClick, children }) {
  console.log(`Rendering button: ${children}`);
  return <button onClick={onClick}>{children}</button>;
}

export default function UseCallbackExample() {
  const [count, setCount] = useState(0);
  const [theme, setTheme] = useState(false);

  // Without useCallback, this function is recreated every render
  const increment = useCallback(() => {
    setCount((c) => c + 1);
  }, []); // Dependencies: none

  return (
    <div style={{ background: theme ? "black" : "white", color: theme ? "white" : "black", padding: "20px" }}>
      <h2>Count: {count}</h2>
      <Button onClick={increment}>Increment</Button>
      <Button onClick={() => setTheme(!theme)}>Toggle Theme</Button>
    </div>
  );
}

</code>
</pre>
<h3>
  Why useCallback Helps Here
</h3>
  

<p>
  Without useCallback, increment is a new function on every render.

If <Button> is memoized with React.memo, it would still re-render because the onClick prop is "different" each time.

With useCallback, the same function reference is reused until dependencies change.

Real-World Practical Example — Search with Debounce

Imagine a search box where the search function is passed down to a child component that handles API calls with debounce.
We don’t want the debounce to reset on every render just because the parent re-rendered.
</p>
  <pre><code class="language-js">

import React, { useState, useCallback } from "react";

function SearchInput({ onSearch }) {
  console.log("Rendering SearchInput...");
  return (
    <input
      type="text"
      placeholder="Search..."
      onChange={(e) => onSearch(e.target.value)}
    />
  );
}

export default function SearchPage() {
  const [results, setResults] = useState([]);

  const fetchResults = useCallback((query) => {
    console.log(`Searching for: ${query}`);
    // Imagine calling an API here
    setResults([`Result for "${query}"`]);
  }, []); // No dependencies → function won't be recreated

  return (
    <div style={{ padding: "20px" }}>
      <h2>Search Page</h2>
      <SearchInput onSearch={fetchResults} />
      <ul>
        {results.map((res, i) => (
          <li key={i}>{res}</li>
        ))}
      </ul>
    </div>
  );
}
</code>
</pre>
<h3>Where useCallback Helps in the Real World</h3>
<p>
Form inputs where change handlers are passed to deeply nested components.

Buttons and menus in large UIs to avoid re-rendering child components unnecessarily.

Debounced or throttled API calls to avoid function recreation resetting timers.

React.memo with functions as props — prevents child components from thinking props have changed.
</p>
<h3>💡 Difference from useMemo:</h3>
<p>
useMemo memoizes a value (result of a calculation).

useCallback memoizes a function (so its reference stays the same).
</p>
</div>
  <h2>Real-World Example: Preventing Child Re-render</h2>
  <pre><code class="language-js">
import React, { useState, useCallback } from "react";

const ChildButton = React.memo(({ onClick }) =&gt; {
  console.log("Child rendered");
  return &lt;button onClick={onClick}&gt;Click Me&lt;/button&gt;;
});

export default function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() =&gt; {
    alert("Button clicked!");
  }, []);

  return (
    &lt;div&gt;
      &lt;h3&gt;Count: {count}&lt;/h3&gt;
      &lt;button onClick={() =&gt; setCount(count + 1)}&gt;Increment&lt;/button&gt;
      &lt;ChildButton onClick={handleClick} /&gt;
    &lt;/div&gt;
  );
}
  </code></pre>
</div>
<div>
  <pre>
    <code class="language-js">
      import React from "react";

const ChildButton = React.memo(({ onClick }) => {
  console.log("Child rendered"); // To check if re-render happens
  return <button onClick={onClick}>Click Me</button>;
});

export default ChildButton;
    </code>
  </pre>
</div>

