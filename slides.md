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

# Today's demo

<a href="https://xstate-talk-todo-app.netlify.app" target="_blank">

![](/Demo.png)

</a>

---

# Structure of the app

- Data fetching (as a side effect) when page is first rendered
- Defensive code to prevent behaviors, based on booleans: Open the form if a condition is met when clicking on *Add a todo* button
- Booleans need to be synced from multiple places (error prone)
- Sometimes, a behavior is so complex to implement that the reason behind it fades out
  - Conditionally calling a debounced version of `synchronizeTodoList`

<!--
Open VSCode with code for /without-xstate page

- Date fetching
- Prevent from opening the form during initial loading
- Need to 
-->

---
layout: intro
---

# It's time for a better world.

A world made of state machines.

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

<a href="https://stately.ai/registry/editor/6fa98cfb-fe39-4479-a6a6-2db09dc872d1?machineId=d530e75e-35d7-4e78-a67f-56f6b64b6eab" target="_blank">

![](/Machine.png)

</a>

---

# At the end... what is XState?

- https://stately.ai/docs (🆕 New documentation released last week!)
- Library to create state machines in JavaScript/TypeScript
- Can be used anywhere JavaScript can run (browser, Node.js, Temporal Workflows, ...)
- In the frontend, it can be used with any framework, and even with no framework at all
- [Stately](https://stately.ai) develops visual tools for XState

---

# Set up XState in the page

- `createMachine` to define a machine
- `useMachine` to interpret a machine and get it's current state
- `state.matches('...')` to know if the machine is in a particular state
- `send(...)` to send an event to the machine

---

# Reimplement with XState

- Logic is centralized in the machine
- Event handlers only forward events – with a payload or not
- View *derives* state from the current state of the machine
- Context is used to store infinite data, like a list of todos

---

# Pros and cons of XState

<br>

<div class="grid grid-cols-2">
<div>

## Pros

- Application logic is centralized and kept apart from implementation details
- Application logic is easier to understand, and even more with visual tools
- The logic is predictable; only what has been defined in the machine can occur
- Non-developers can understand the code – and even participate to it!

</div>

<div>

## Cons

- Learning curve with statecharts, and with XState API
- Not a paradigm wildly adopted in front-end development; might take some efforts to convert a team to it

</div>
</div>

---

# Other things XState can do

- Libraries for all front-end frameworks: @xstate/react, @xstate/vue, @xstate/svelte, @xstate/solid (coming soon)
- [@xstate/inspect](https://stately.ai/docs/tools/inspector): Inspect a machine in real-time and interact with it
- [@xstate/test](https://stately.ai/docs/xstate/packages/xstate-test): Generate tests from a state machine (aka. Model-Based Testing)
- [Power a CLI built with Node.js](https://github.com/mattpocock/matt-cli/blob/13953df17e05213bff1f69fb1e9e416a3996171a/src/pr.ts)
- [In a Node.js server](https://youtu.be/qqyQGEjWSAw)
- [Power a Temporal Workflow](https://github.com/Devessier/temporal-electronic-signature)
- Soon... with other languages than JavaScript

---

# To learn more

- https://stately.ai
- https://statecharts.dev

---

# Thank you!

Thank you Cédric and Loïck for inviting me.

<div class="grid grid-cols-2 gap-12">

<div>

- <mdi-github /> [Devessier](https://github.com/Devessier)
- <mdi-link /> [baptiste.devessier.fr](https://baptiste.devessier.fr)
- <mdi-twitter /> [BDevessier](https://twitter.com/BDevessier)
- Code: https://github.com/Devessier/xstate-talk-demo
- Slides: https://xstate-talk.netlify.app

</div>

<div class="w-52">

Link to GitHub repo 👇

![](/qr-code.png)

</div>

</div>


