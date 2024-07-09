# React for Vue Developers
## React Basics and Similarities to Vue 
### 0. React Components and JSX

React components can be written as JavaScript functions. They return what gets rendered to the DOM. JSX allows you to write HTML-like code in your JavaScript.

```jsx
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}
```

### 1. Binding to Attributes and Interpolating Values

In both React and Vue, it's common to bind state to attributes and interpolate it in the UI.

Here's how you can do it in React with Hooks:

```jsx
import React, { useState } from "react";

function MyComponent() {
  const [text, setText] = useState("Hello, world!");
  const [color, setColor] = useState("red");

  return (
    <div>
      <h1 style={{ color: color }}>{text}</h1>
      <input 
        type="text" 
        value={text} 
        onChange={e => setText(e.target.value)} 
        placeholder="Change the text"
      />
      <input 
        type="color" 
        value={color} 
        onChange={e => setColor(e.target.value)} 
      />
    </div>
  );
}
```

In this example, the `text` state is interpolated in the `<h1>` tag, and the `color` state is bound to the `style` attribute of the `<h1>` tag. The `<input>` elements allow you to modify these states, and their `value` attributes are bound to the corresponding states. The `onChange` event handlers update the state when you type in the text field or select a color.

And here's a similar example in Vue using the Composition API and the `<script setup>` syntax:

```vue
<template>
  <div>
    <h1 :style="{ color: color }">{{ text }}</h1>
    <input 
      type="text" 
      v-model="text" 
      placeholder="Change the text"
    />
    <input 
      type="color" 
      v-model="color" 
    />
  </div>
</template>

<script setup>
import { ref } from 'vue';

const text = ref('Hello, world!');
const color = ref('red');
</script>
```

In this Vue example, the `text` state is interpolated in the `<h1>` tag and the `color` state is bound to the `style` attribute of the `<h1>` tag. The `v-model` directive creates two-way data binding between the `<input>` elements and the corresponding states, allowing you to modify the states and see the updates in real time.

### 2. React Hooks and their parallels to Vue's Composition API

**useState vs ref**

`useState` in React is used to create reactive variables, similar to `ref` in Vue.

React:
```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

Vue:
```vue
<template>
  <div>
    <p>You clicked {{ count }} times</p>
    <button @click="increment">
      Click me
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const count = ref(0);

const increment = () => {
  count.value++;
};
</script>
```

**useEffect vs watchEffect**

`useEffect` in React is used to perform side effects when reactive variables change, similar to `watchEffect` in Vue.

React:
```jsx
import { useState, useEffect } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

Vue:
```vue
<template>
  <div>
    <p>You clicked {{ count }} times</p>
    <button @click="increment">
      Click me
    </button>
  </div>
</template>

<script setup>
import { ref, watchEffect } from 'vue';

const count = ref(0);

watchEffect(() => {
  document.title = `Count: ${count.value}`;
});

const increment = () => {
  count.value++;
};
</script>
```

### 3. Advanced Hooks and their parallels in Vue

**useContext**

`useContext` in React is used to share global state and avoid prop drilling, similar to provide/inject in Vue.

**useReducer vs useStore**

`useReducer` in React is used to handle complex state transitions, similar to using a store in Pinia.

React:
```jsx
import { useReducer } from 'react';

const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  
  return (
    <div>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
    </div>
  );
}
```

Vue (using Pinia):
```vue
<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">+</button>


    <button @click="decrement">-</button>
  </div>
</template>

<script setup>
import { useStore } from 'pinia';

const store = useStore();

const count = computed(() => store.state.count);

const increment = () => {
  store.increment();
};

const decrement = () => {
  store.decrement();
};
</script>
```

### 4. Custom Hooks vs Composable Functions

Custom Hooks in React are a mechanism to reuse stateful logic, similar to Composable Functions in Vue.

React:
```jsx
import { useState } from 'react';

function useCounter() {
  const [count, setCount] = useState(0);
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);

  return { count, increment, decrement };
}

function Counter() {
  const { count, increment, decrement } = useCounter();
  
  return (
    <div>
      Count: {count}
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}
```

Vue:
```vue
<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const count = ref(0);
const increment = () => count.value++;
const decrement = () => count.value--;

</script>
```

### 5. Performance Optimization in React

React provides several mechanisms to optimize performance:

**React.memo**

Similar to Vue's `memo`, `React.memo` can help prevent unnecessary renders of your components by only re-rendering if the props change.

**useMemo and useCallback**

These hooks allow you to avoid expensive computations on every render by memoizing the result and only re-calculating if dependencies change. This is similar to Vue's computed properties.

React:
```jsx
import React, { useState, useMemo, useCallback } from 'react';

function ExpensiveComponent({ compute, value }) {
  return <div>Computed value: {compute(value)}</div>;
}

function App() {
  const [input, setInput] = useState('');

  const compute = useCallback((val) => {
    let result = 0;
    for (let i = 0; i < 1000000000; i++) {
      result += val * i;
    }
    return result;
  }, []);

  const value = useMemo(() => compute(Number(input)), [input, compute]);

  return (
    <div>
      <input value={input} onChange={(e) => setInput(e.target.value)} />
      <ExpensiveComponent compute={compute} value={value} />
    </div>
  );
}
```

Vue:
```vue
<template>
  <div>
    <input v-model="input" />
    <ExpensiveComponent :compute="compute" :value="value" />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const input = ref('');

const compute = (val) => {
  let result = 0;
  for (let i = 0; i < 1000000000; i++) {
    result += val * i;
  }
  return result;
};

const value = computed(() => compute(Number(input.value)));
</script>
```

### 6. React Router

React Router is commonly used for routing in React applications, similar to vue-router in Vue.js.

React:
```jsx
import React from "react";
import { BrowserRouter as Router, Switch, Route

, Link } from "react-router-dom";

function App() {
  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about">About</Link>
            </li>
          </ul>
        </nav>

        <Switch>
          <Route path="/about">
            <About />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

function Home() {
  return <h2>Home</h2>;
}

function About() {
  return <h2>About</h2>;
}
```

Vue:
```vue
<template>
  <div>
    <nav>
      <ul>
        <li>
          <router-link to="/">Home</router-link>
        </li>
        <li>
          <router-link to="/about">About</router-link>
        </li>
      </ul>
    </nav>

    <router-view></router-view>
  </div>
</template>
```

### 7. Conclusion

This guide provides a detailed overview of idiomatic React for someone with a background in Vue.js, using the Composition API and Pinia for state management. React's functional components and hooks provide a powerful and flexible way to build UIs. 

React also has a robust ecosystem with libraries for virtually every use case. For state management, besides the Context API and useReducer, you might consider Redux or MobX. For data fetching, there's react-query or Apollo Client (for GraphQL). For UI components, there's Material-UI, Ant Design, and Chakra UI.

The React community is active and helpful, so if you ever find yourself stuck or not sure how to make your code more idiomatic, don't hesitate to seek help from the community.

### 8. Testing in React

Testing in React is primarily done using a combination of Jest as the test runner and React Testing Library for interacting with your components in tests. This parallels Vue's use of Jest and Vue Testing Library.

**Example with React:**
```jsx
import { render, fireEvent, screen } from '@testing-library/react';
import Counter from './Counter';

test('renders counter', () => {
  render(<Counter />);
  const linkElement = screen.getByText(/You clicked 0 times/i);
  expect(linkElement).toBeInTheDocument();
});

test('increments counter', () => {
  render(<Counter />);
  const button = screen.getByText('Click me');
  fireEvent.click(button);
  const linkElement = screen.getByText(/You clicked 1 times/i);
  expect(linkElement).toBeInTheDocument();
});
```

**Example with Vue:**
```vue
import { render, fireEvent } from '@testing-library/vue';
import Counter from './Counter.vue';

test('renders counter', async () => {
  const { getByText } = render(Counter);
  expect(getByText('You clicked 0 times')).toBeInTheDocument();
});

test('increments counter', async () => {
  const { getByText } = render(Counter);
  const button = getByText('Click me');
  await fireEvent.click(button);
  expect(getByText('You clicked 1 times')).toBeInTheDocument();
});
```

### 9. Accessibility in React

React provides built-in support for adding accessibility attributes to your components. This is similar to how you'd add accessibility features in Vue - by adding the appropriate HTML attributes.

React:
```jsx
function AccessibleButton({ onClick, children }) {
  return (
    <button onClick={onClick} aria-label="descriptive text">
      {children}
    </button>
  );
}
```

Vue:
```vue
<template>
  <button @click="onClick" :aria-label="label">
    <slot></slot>
  </button>
</template>

<script setup>
const onClick = () => {};
const label = "descriptive text";
</script>
```

### 10. Form Handling in React

Handling forms in React can be done using controlled components, where the form data is handled by the state within the component. This is similar to v-model binding in Vue.

React:
```jsx
import { useState } from 'react';

function SignupForm() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSubmit = (event) => {
    event.preventDefault();
    // handle form submission
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Email:
        <input type="email" value={email} onChange={(e) => setEmail(e.target.value)} />
      </label>
      <label>
        Password:
        <input type="password" value={password} onChange={(e) => setPassword(e.target.value)} />
      </label>
      <input type="submit" value="Submit" />
    </form>
  );
}
```

Vue:
```vue
<template>
  <form @submit.prevent="handleSubmit">
    <label>
      Email:
      <input type="email" v-model="email" />
    </label>
    <label>
      Password:
      <input type="password" v-model="password" />
    </label>
    <input type="submit" value="Submit" />
  </form>
</template>

<script setup>
import { ref } from 'vue';

const email = ref("");
const password = ref("");

const handleSubmit = () => {
  // handle form submission
};
</script>
```

## Using TypeScript

### 0. Component Structure

In TypeScript, you can use interfaces or types to specify the props that a component accepts.

React:
```tsx
interface ButtonProps {
  onClick: () => void;
  children: React.ReactNode;
}

const Button: React.FC<ButtonProps> = ({ onClick, children }) => (
  <button onClick={onClick}>{children}</button>
);
```

Vue:
```vue
<script setup lang="ts">
import { defineProps } from 'vue';

interface ButtonProps {
  onClick: () => void;
}

defineProps<ButtonProps>();
</script>
```

### 2. State Management

For state management, you can provide a type when calling useState (React) or ref (Vue).

React:
```tsx
const [count, setCount] = useState<number>(0);
```

Vue:
```vue
<script setup lang="ts">
import { ref } from 'vue';

const count = ref<number>(0);
</script>
```

### 3. Lifecycle Methods

TypeScript will ensure that you're using the correct type of value in lifecycle methods.

React:
```tsx
useEffect(() => {
  // effect
  return () => {
    // cleanup
  };
}, [/* dependencies */]);
```

Vue:
```vue
<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue';

onMounted(() => {
  // effect
});

onUnmounted(() => {
  // cleanup
});
</script>
```

### 4. Custom Hooks (React) or Composables (Vue)

TypeScript will ensure that the parameters and return values from hooks or composables are of the correct type.

React:
```tsx
function useCounter(initialValue: number): [number, () => void] {
  const [count, setCount] = useState(initialValue);
  const increment = () => setCount(count + 1);
  return [count, increment];
}
```

Vue:
```vue
<script setup lang="ts">
import { ref } from 'vue';

function useCounter(initialValue: number): { count: Ref<number>; increment: () => void; } {
  const count = ref(initialValue);
  const increment = () => count.value++;
  return { count, increment };
}
</script>
```

### 5. Context (React) or Provide/Inject (Vue)

TypeScript can help ensure that you're providing and consuming values of the correct type.

React:
```tsx
interface CountContextProps {
  count: number;
  increment: () => void;
}

const CountContext = createContext<CountContextProps | undefined>(undefined);

// ensure that the context value is of the correct type
<CountContext.Provider value={{ count, increment }} />
```

Vue:
```vue
<script setup lang="ts">
import { provide, ref } from 'vue';

interface CountContextProps {
  count: Ref<number>;
  increment: () => void;
}

const count = ref(0);
const increment = () => count.value++;

// types are inferred when providing
provide('count', count);
provide('increment', increment);
</script>
```

### 6. Routing

TypeScript can help ensure that the correct parameters are passed to routes.

React:
```tsx
<Route path="/post/:id">
  <Post />
</Route>

// in Post component
const { id } = useParams<{ id: string }>();
```

Vue:
```vue
<script setup lang="ts">
import { useRoute } from 'vue-router';

const route = useRoute();
const id = route.params.id as string;
</script>
```

The rest of the points (7 to 15) would have similar TypeScript adaptations. In general, TypeScript provides a way to

ensure type safety and autocompletion, making it easier to write correct code and to refactor with confidence. 

### 7. Events and Event Handlers

React:
```tsx
interface ButtonProps {
  onClick: (event: React.MouseEvent<HTMLButtonElement>) => void;
}

const Button: React.FC<ButtonProps> = ({ onClick }) => (
  <button onClick={onClick}>Click me</button>
);
```

Vue:
```vue
<script setup lang="ts">
import { defineProps } from 'vue';

interface ButtonProps {
  onClick: (event: MouseEvent) => void;
}

defineProps<ButtonProps>();
</script>
```

### 11. Prop Types and Default Props

TypeScript can be used to enforce type safety for props in both React and Vue.

React:
```tsx
interface GreetingProps {
  name?: string;
}

const Greeting: React.FC<GreetingProps> = ({ name = 'Stranger' }) => (
  <h1>Hello, {name}</h1>
);
```

Vue:
```vue
<script setup lang="ts">
import { defineProps, withDefaults } from 'vue';

const props = withDefaults(defineProps<{
  name?: string;
}>(), {
  name: 'Stranger'
});
</script>
```

### 12. Error Boundaries

In TypeScript, you can specify the types of error and error info objects.

React:
```tsx
class ErrorBoundary extends React.Component<{}, { hasError: boolean }> {
  // rest of the code
}

```

Vue: 
```vue
<script setup lang="ts">
import { ref } from 'vue';

const hasError = ref<boolean>(false);

errorCaptured((err: Error, instance: ComponentPublicInstance, info: string) => {
  logErrorToMyService(err, info);
  hasError.value = true;
  return true; // stop error propagation
});
</script>
```

### 13. Context API for Global State Management

For global state management, TypeScript can ensure that the correct types are provided and injected.

React:
```tsx
interface CountContextType {
  count: number;
  setCount: React.Dispatch<React.SetStateAction<number>>;
}

const CountContext = React.createContext<CountContextType | undefined>(undefined);

// usage within a component
const { count, setCount } = useContext(CountContext) as CountContextType;
```

Vue:
```vue
<script setup lang="ts">
import { provide, ref, inject } from 'vue';

const count = ref<number>(0);
const increment = () => count.value++;

provide('count', count);
provide('increment', increment);

// within another component
const count = inject<Ref<number>>('count')!;
const increment = inject<() => void>('increment')!;
</script>
```

### 14. Server Side Rendering (SSR) with Next.js

In TypeScript, you can specify the types of the context object and the returned props.

React with Next.js:
```tsx
export async function getServerSideProps(context: NextPageContext) {
  const res = await fetch('https://api.example.com/data');
  const data: DataType = await res.json();

  if (!data) {
    return {
      notFound: true,
    };
  }

  return {
    props: { data }, // will be passed to the page component as props
  };
}
```

Vue with Nuxt.js:
```vue
<script lang="ts">
export default {
  async asyncData({ params }: Context) {
    const res = await fetch('https://api.example.com/data');
    const data: DataType = await res.json();
    return { data };
  }
};
</script>
```

### 15. Data Fetching

When fetching data an early return can be used to display a loading component instead of v-if + v-else.

React:
```tsx
function Loading() {
  return <div>Loading...</div>
}

function App() {
  const [loading,setLoading] = useState(true);
  const [data,setData] = useState<string | null>(null);

  const fetchData = async (): Promise<string> => {
    // fetch data
  };

  useEffect(async () => {
    setData(await fetchData());
    setLoading(false);
  }, [])

  if(loading)
    return <Loading />;

  return <div>{data}</div>;
}
```

Vue:
```vue
<template>
  <div v-if="loading">Loading...</div>
  <div v-else>{{ data }}</div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const fetchData = async (): Promise<string> => {
  // fetch data
};

const loading = ref(true);
const data = ref<string | null>(null);

onMounted(async () => {
  data.value = await fetchData();
  loading.value = false;
});
</script>
```

With TypeScript, you can strongly type your components, state, props, events, and more. This can help catch errors during development and provide a better developer experience with autocompletion and inline documentation. Additionally, TypeScript can help with refactoring, making it easier to make significant changes with confidence that you're not breaking existing functionality.
