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

