---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Intro to Deployment
Unit 07 - Lesson 05

- [ ] Typescript
- [ ] react-testing-library / vitest


<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
-->

---
transition: slide-left
---

# Recap 

- Lab yesterday we covered Docker
- `docker-compose pull && docker-compose up`
- `docker compose watch`
- `docker image prune -a`
- `docker volume rm $(docker volume ls -q)`
- on my Mac I had to edit Dockerfile to include:
  ```yaml
  backend:
    volumes:
      - ./backend/src:/usr/local/app/src
      - ./backend/package.json:/usr/local/app/package.json
  ```
  ```yaml
  client:
      volumes:
      - ./client/src:/usr/local/app/src
      - ./client/package.json:/usr/local/app/package.json
  ```

---
transition: slide-left
---

# Typescript

- Benefits of JS
- Tip#1: Always hover over your variables to see what type it is (as defined previously by TS)
- Exercise: create new vite/react/TS project
  - Extract `<h1>` into its own component
  - Let's play with its props and fix it with TS
  - Next few slides will give us material to play with

---
transition: slide-left
---

# Typescript: Primitive Types
see https://www.typescriptlang.org/

| Type         | Category       | Description                                             | Example                                 |
|--------------|----------------|---------------------------------------------------------|-----------------------------------------|
| `number`     | Primitive      | Represents all numbers (integers, floats)               | `let age: number = 30;`                 |
| `string`     | Primitive      | Textual data                                            | `let name: string = "Alice";`           |
| `boolean`    | Primitive      | True or false                                           | `let isActive: boolean = true;`         |
| `null`       | Primitive      | Represents no value (explicit null)                     | `let nothing: null = null;`             |
| `undefined`  | Primitive      | Value not assigned                                      | `let x: undefined = undefined;`         |


---
transition: slide-left
---

# Primitive Types (pg.2)

| Type         | Category       | Description                                             | Example                                 |
|--------------|----------------|---------------------------------------------------------|-----------------------------------------|
| `object`     | Built-in       | Non-primitive types                                     | `let obj: object = { key: "value" };`   |
| `Array<T>`   | Built-in       | Typed list of values                                    | `let nums: Array<number> = [1, 2, 3];`  |
| `function`   | Type Variant   | Describes a function signature                          | `let add: (a: number, b: number) => number;` |
| `any`        | Built-in       | Opt-out of type checking (avoid if possible)            | `let data: any = 5; data = "text";`     |


---
transition: slide-left
---

# Primitive types (pg.3)

| Type         | Category       | Description                                             | Example                                 |
|--------------|----------------|---------------------------------------------------------|-----------------------------------------|
| `unknown`    | Built-in       | Like `any` but safer – must check before using          | `let value: unknown = "hello";`         |
| `void`       | Built-in       | Used when function returns nothing                      | `function log(): void { console.log() }`|
| `never`      | Built-in       | Value that never occurs (e.g. function that throws)     | `function fail(): never { throw "err"; }`|
| `Tuple`      | Built-in       | Fixed-length array with specific types                  | `let tuple: [string, number] = ["a", 1];`|



---
transition: slide-left
---

# Primitive types (pg.4)

| Type         | Category       | Description                                             | Example                                 |
|--------------|----------------|---------------------------------------------------------|-----------------------------------------|
| `enum`       | Built-in       | Named constants (numeric or string values)              | `enum Color { Red, Green, Blue }`       |
| `literal`    | Type Variant   | Exact value types                                       | `let direction: "left" | "right";`      |
| `union`      | Type Variant   | Variable can be one of several types                    | `let id: number | string;`              |
| `intersection`| Type Variant  | Combines multiple types                                 | `type A = B & C;`                        |

---
transition: slide-left
---

# Zod

- see https://zod.dev
- a TS-first schema validation with static type inference
- can define schemas you can use to validate data

```ts
import * as z from "zod";
 
const User = z.object({
  name: z.string(),
});
 
// some untrusted data...
const input = { /* stuff */ };
 
// the parsed result is validated and type safe!
const data = User.parse(input);
 
// so you can use it with confidence :)
console.log(data.name);
```

---
transition: slide-left
---

# Exercise 1
Use literal and union types to model a simple traffic light system.

1. Define a type alias called TrafficLightColor that allows only "red", "yellow", or "green".
2. Create a function called getNextLight that:
  - Takes a parameter of type TrafficLightColor
  - Returns the next light color in the sequence
    - red → green → yellow → red (loop)
3. Use the function with a variable typed as TrafficLightColor.

## Hints:
- Use a literal union type: "red" | "green" | "yellow"
- You can return a literal type too

```ts
getNextLight("red"); // "green"
getNextLight("green"); // "yellow"
```



---
layout: image-right
transition: slide-left
image: /assets/matt.png
backgroundSize: 400px 300px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- ⌨️ [Total Typescript](https://www.totaltypescript.com/)
- 💯 [react testing library](https://testing-library.com/docs/react-testing-library/intro/)
- 🛹 [Scrimba: Learn TS](https://scrimba.com/learn-typescript-c0l)

<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)

---
transition: slide-left
---

# React Testing Library
see https://testing-library.com/

- Test 1: Does heading render correctly from child component?

```tsx
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders the heading from the Heading component', () => {
  render(<App />);
  const heading = screen.getByRole('heading', { name: /vite \+ react/i });
  expect(heading).toBeInTheDocument();
});
```

---
transition: slide-left
---

# React Testing Library

- Does logo render with correct alt text

```tsx
test('renders the Vite and React logos', () => {
  render(<App />);
  
  expect(screen.getByAltText(/vite logo/i)).toBeInTheDocument();
  expect(screen.getByAltText(/react logo/i)).toBeInTheDocument();
});
```

---
transition: slide-left
---

# React Testing Library

- Does clicking the button increase the count?
```tsx
import userEvent from '@testing-library/user-event';

test('increments count when button is clicked', async () => {
  render(<App />);
  const button = screen.getByRole('button', { name: /count is 0/i });

  await userEvent.click(button);
  expect(button).toHaveTextContent('count is 1');

  await userEvent.click(button);
  expect(button).toHaveTextContent('count is 2');
});
```

---
transition: slide-left
---

# Homework

- Think about your Capstone (planning phase) 