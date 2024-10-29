# A Guide to Basic React Hooks

*React hooks provide a powerful way to use state and lifecycle features in functional components. In this guide, we’ll go through the core hooks: useState, useEffect, useContext, useReducer, and useRef.*

---

### Understanding Hooks

**Hooks**  in React are special functions that let you use React features like state and lifecycle methods in functional components. Before hooks, only class components could have state and lifecycle features. Hooks were introduced to make React code simpler and more reusable by allowing functional components to do everything that class components can.

---

**Why Use Hooks**  
- Simplicity: Hooks make it easy to add state and side effects to functional components.
- Reuse Logic: With hooks, you can extract reusable logic into separate functions (custom hooks).
- Easier to Read and Maintain: Functional components with hooks are often easier to understand and manage, especially in complex applications..

---

#### 1. `useState` Hook

- Purpose: Manages local state in functional components.

`Syntax`:

    const [state, setState] = useState(initialState);

`Example: A simple counter`

    import React, { useState } from 'react';

     function Counter() {
      const [count, setCount] = useState(0);

      const increment = () => setCount(count + 1);
  
      return (
      <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      </div>
    );
    }
    export default Counter;


#### 2. `useEffect` Hook

- Purpose: Handles side effects, such as data fetching, subscriptions, or manually changing the DOM.

`Syntax`:

    useEffect(() => {
    // effect logic here

    return () => {
    // cleanup (optional)
    };
    }, [dependencies]);


`Example: Fetching data on component mount`

    import React, { useState, useEffect } from 'react';

    function DataFetcher() {
     const [data, setData] = useState([]);

    useEffect(() => {
    fetch('https://api.example.com/data')
      .then((response) => response.json())
      .then((json) => setData(json));
    }, []);

    return (
    <ul>
      {data.map((item, index) => (
        <li key={index}>{item.name}</li>
      ))}
    </ul>
    );
    }
    export default DataFetcher;

#### 3. `useContext` Hook

- Purpose: Allows components to access context values directly, avoiding the need for props drilling.

`Syntax`:

    const value = useContext(MyContext);


`Example: Using a theme context`

    import React, { useContext } from 'react';

    const ThemeContext = React.createContext('light');

    function ThemedComponent() {
     const theme = useContext(ThemeContext);

     return <div style={{ background: theme === 'light' ? '#fff' : '#333' }}>Current Theme: {theme}</div>;
    }

    function App() {
     return (
    <ThemeContext.Provider value="dark">
      <ThemedComponent />
    </ThemeContext.Provider>
     );
    }
    export default App;

`Explanation`:`useContext` allows `ThemedComponent` to access `ThemeContext` directly without needing to pass it down through props

#### 4. `useReducer` Hook

- Purpose: Manages complex state logic and is an alternative to useState, especially for multiple sub-values or complex state transitions.

`Syntax`:

const [state, dispatch] = useReducer(reducer, initialArg, init);


`Example: Counter with useReducer`

    import React, { useReducer } from 'react';

    const initialState = 0;

    const reducer = (state, action) => {

    switch (action.type) {
    case 'increment':
      return state + 1;
    case 'decrement':
      return state - 1;
    default:
      return state;
     }
    };

     function Counter() {
        
     const [count, dispatch] = useReducer(reducer, initialState);

    return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
    </div>
    );
    }
    export default Counter;

`Explanation`: `useReducer` uses a reducer function to update the state based on dispatched action

#### 5. `useRef` Hook

- Purpose:  Holds mutable values across renders without causing re-renders. Commonly used to reference DOM elements or hold values between renders.

`Syntax`:

const refContainer = useRef(initialValue);

`Example: Counter with useReducer`

    import React, { useRef } from 'react';

    function FocusInput() {
     const inputRef = useRef(null);

     const handleFocus = () => {
      if (inputRef.current) {
       inputRef.current.focus();
      }
     };

    return (
      <div>
        <input ref={inputRef}type="text"placeholder="Focus me!"/>
        <button onClick={handleFocus}>Focus Input</button>
      </div>
    );
    }
    export default FocusInput;


`Explanation`: `inputRef` holds a reference to the input element, allowing direct DOM manipulation


`Conclusion`:
This guide covered React’s core hooks with examples. Mastering these hooks will allow you to build dynamic, stateful components in functional React applications.
