---
theme: apple-basic
highlighter: shiki
title: What is XState?

layout: intro
---

# What is XState?

Is it a new version of X-Files or X-Men? 🤔

<div class="absolute bottom-10">
  <span class="font-700">
    Baptiste Devessier - 2023
  </span>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Structure of a usual React page

[Todo list app](https://xstate-talk-todo-app.netlify.app)

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

[Fetching machine example](https://stately.ai/registry/editor/6fa98cfb-fe39-4479-a6a6-2db09dc872d1?machineId=d530e75e-35d7-4e78-a67f-56f6b64b6eab)

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

---

# Pros and cons of XState

<br>

<div class="grid grid-cols-2">
<div>

## Pros

- Application logic is centralized and kept apart from implementation details
- Application logic is easier to understand, and even more with visual tools
- The logic is predictable; only what has been defined in the machine can occur
- Non-developers can understand the code – and even participate to it

</div>

<div>

## Cons

- Learning curve with statecharts, and with XState API

</div>
</div>

---

# Other things XState can do

- Libraries for all front-end frameworks: @xstate/react, @xstate/vue, @xstate/svelte, @xstate/solid (coming soon)
- [@xstate/inspect](https://stately.ai/docs/tools/inspector): Inspect a machine in real-time and interact with it
- [@xstate/test](https://stately.ai/docs/xstate/packages/xstate-test): Generate tests from a state machine (aka. Model-Based Testing)
- [Power a CLI built with Node.js](https://github.com/mattpocock/matt-cli/blob/13953df17e05213bff1f69fb1e9e416a3996171a/src/pr.ts)
- [In a Node.js server](https://youtu.be/qqyQGEjWSAw)
- [Power a Temporal Worker](https://github.com/Devessier/temporal-electronic-signature)
- Soon... with other languages than JavaScript

---

# To learn more

- https://stately.ai
- https://statecharts.dev

---

# Thank you!

- <mdi-github /> [Devessier](https://github.com/Devessier)
- <mdi-link /> [baptiste.devessier.fr](https://baptiste.devessier.fr)
- <mdi-twitter /> [BDevessier](https://twitter.com/BDevessier)
- Code: https://github.com/Devessier/xstate-talk-demo
<!-- - Slides: [temporal-electronic-signature-talk.netlify.app](https://temporal-electronic-signature-talk.netlify.app/1) -->
