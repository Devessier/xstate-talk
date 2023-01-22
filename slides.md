---
theme: apple-basic
highlighter: shiki
title: What is XState?

layout: intro
---

# What is XState?

Is it a new version of X-Files or X-Men? 🤔

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Structure of a usual React page

Example: https://xstate-talk-todo-app.netlify.app

- Data fetching when page is first rendered
- Using booleans to keep track of *states*
- Defensive code to prevent behaviors based on booleans
- Difficult to spot the most important lines of code

<!-- Open VSCode with code for /without-xstate page -->

---

## Data fetching

<br>

```tsx
const [todos, setTodos] = useState<TodoItem[]>([]);
const [hasFetchedInitialTodos, setHasFetchedInitialTodos] = useState(false);

useEffect(() => {
  fetchInitialTodos()
    .then((initialTodos) => {
      setTodos(initialTodos);
    })
    .catch(() => {
      setTodos([]);
    })
    .finally(() => {
      setHasFetchedInitialTodos(true);
    });
}, []);
```

---

## Defensive code based on current state

<br>

```tsx
const [hasFetchedInitialTodos, setHasFetchedInitialTodos] = useState(false);
const [isTodoCreationFormOpen, setIsTodoCreationFormOpen] = useState(false);

const isLoadingInitialTodos = hasFetchedInitialTodos === false;

function handleOpenTodoCreation() {
  if (isLoadingInitialTodos === true) {
    return;
  }

  setIsTodoCreationFormOpen(true);
}
```

---

## Different kinds of code are mixed

Code to keep track of the current state is mixed with code actually starting things.

```tsx
function handleSaveTodo(newTodo: string) {
  setTodos((todoList) => [
    ...todoList,
    {
      id: generateId(),
      label: newTodo,
      checked: false,
    },
  ]);

  setIsTodoCreationFormOpen(false);

  askForSynchronization();
}
```

---

## Calling conditionnally a debounced function is... hard

```tsx
const onSynchronize = useEvent(() => {
  return synchronizeTodoList(todos);
});

const debouncedSynchronization = useMemo(
  () =>
    debounce(async () => {
      try {
        setIsSynchronizing(true);

        await onSynchronize();
      } catch (message) {
        return console.error(message);
      } finally {
        setIsSynchronizing(false);
      }
    }, 1_000),
  [onSynchronize]
);

const askForSynchronization = useCallback(() => {
  // Do not call the call the function while a synchronization is already occuring.
  if (isSynchronizing === true) {
    return;
  }

  debouncedSynchronization();
}, [debouncedSynchronization, isSynchronizing]);
```

---
layout: intro
---

# It's time for a better world.

And this world is made of state machines.

---

# What are state machines?

- Finite number of *states*
- In exactly one state at a time
- An *initial* state
- Wait for *events*
- Events trigger *transitions* from one state to another one
- Transitions are *deterministic* (same sequence of events always gives the same result)

---

# State machine to fetch data

<img src="/CleanShot 2023-01-22 at 16.33.05@2x.png" />

---

# At the end... what is XState?

- https://stately.ai/docs (🆕 New documentation released last week!)
- Library to create state machines in JavaScript/TypeScript
- Can be used anywhere JavaScript can run (browser, Node.js, Temporal Workflows, ...)
- In the frontend, it can be used with any framework, and even with no framework at all
- [Stately](https://stately.ai) develops visual tools for XState

---

# Set up XState

<br>

## Install xstate and @xstate/react

```bash
npm install xstate @xstate/react
```

<br>

## Create a machine

```ts
import { createMachine } from 'xstate'

const todoMachine = createMachine()
```

<br>

## Use the machine in a React component

```ts
import { useMachine } from '@xstate/react'
import { todoMachine } from '~/machines/todo'

function Component() {
  const [state, send] = useMachine(todoMachine)
}
```

---

# Reimplement the logic
