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
- `docker compose down`
- `docker-compose pull && docker-compose up` or `docker compose watch`
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

# View a more complex use of TS

- ChatGPT: `give a sample tsx that uses typescript in a more complex way inside a react component`

---
transition: slide-left
---

# Common Use-case: Using TS inside a .map function

```tsx
type User = { id: number; name: string;};

type UserSummary = { displayName: string; };

type UserListProps = { users: User[]; };

const UserList: React.FC<UserListProps> = ({ users }) => {
  const userSummaries: UserSummary[] = users.map((user): UserSummary => {
    return {
      displayName: `User: ${user.name}`
    };
  });

  return (
    <ul>
      {userSummaries.map((summary, index) => (
        <li key={index}>{summary.displayName}</li>
      ))}
    </ul>
  );
};
export default UserList;
```

---
transition: slide-left
---

# Exercise

- Git clone https://github.com/avcoder/ts-exercise-01
- Transform the entire thing to TS
  - Look for TS errors in VS Code in lower right corner
- Stretch goals:
  - Create a `FilterType` that has a union of 'all', 'complete', 'incomplete'
  - Extract into components: 
    - TaskItem
    - TaskList



---
transition: slide-left
---

# Zod
see https://zod.dev

- a TS-first schema validation with static type inference 
- works on runtime (TS works at compile time only)
- use to: validate API inputs or responses, reading JSON or user input, etc.
- TS = spell checker while writing; Zod = proofreader catching real mistakes in the essay

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

# Why TypeScript alone may not be enough

- TypeScript is great for development-time type checking. It ensures that your code should be correct — but it doesn’t exist at runtime. Once compiled to JavaScript, all types are erased.
- So if you get data from:
  - an API request (incoming JSON)
  - user input
  - environment variables
  - or any untyped source
- TypeScript won’t verify that data. You need runtime validation for that — and that's where Zod comes in.

```ts
function greet(user: { name: string }) {
  console.log("Hello " + user.name);
}

greet(JSON.parse('{"name":123}')); // TS thinks this is fine!
```

---
transition: slide-left
---

# TS vs Zod comparison chart

| Feature                                     | TypeScript        | Zod               |
| ------------------------------------------- | ----------------- | ----------------- |
| Compile-time type checking                  | ✅                 | ✅ (via `z.infer`) |
| Runtime type validation                     | ❌                 | ✅                 |
| Automatic error messages                    | ❌                 | ✅                 |
| Custom validation rules                     | 🚫 (not directly) | ✅                 |
| Schema-based parsing                        | ❌                 | ✅                 |
| Safer API handling (e.g. `fetch` responses) | ❌                 | ✅                 |
| Works with JSON and external data           | ❌                 | ✅                 |

---
transition: slide-left
---

# Exercise using Zod

```ts
import { z } from "zod";

const UserSchema = z.object({ // 1. Define the schema
  name: z.string().min(2),
  email: z.string().email(),
  age: z.number().gte(18),
});

const validUser = { 
  name: "Alice",
  email: "alice@example.com",
  age: 30,
};

try {
  const user = UserSchema.parse(validUser); // 2. Use `.parse()` for a valid user
  console.log("✅ Valid user:", user);
} catch (e) {
  console.error("❌ Validation error (validUser):", e);
}

type User = z.infer<typeof UserSchema>;  // 3. Infer the type
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

# Most common screen methods

| Method                   | Purpose                                                              |
| ------------------------ | -------------------------------------------------------------------- |
| `getByText()`            | Find an element with exact visible text                              |
| `getByLabelText()`       | Match by `<label>` text (good for form inputs)                       |
| `getByPlaceholderText()` | Match inputs by their placeholder                                    |
| `getByAltText()`         | Match images or media by `alt` text                                  |
| `getByTestId()`          | Match by `data-testid` attribute (fallback, not preferred)           |
| `queryBy...`             | Same as `getBy`, but returns `null` instead of throwing if not found |
| `findBy...`              | For async elements (returns a Promise, waits for element to appear)  |
| `getAllBy...`            | Returns all matching elements (array), throws if none found          |

---
transition: slide-left
---

# Most common assertions

| Matcher                            | Checks...                            |
| ---------------------------------- | ------------------------------------ |
| `toBeInTheDocument()`              | Element is present in the DOM        |
| `toBeVisible()`                    | Element is visible                   |
| `toHaveTextContent()`              | Matches element's text content       |
| `toHaveAttribute()`                | Checks if an attribute exists/equals |
| `toHaveValue()`                    | Checks input/select/textarea value   |
| `toBeDisabled()` / `toBeEnabled()` | Form elements' interactivity         |
| `toHaveClass()`                    | Checks class names                   |


---
transition: slide-left
---

# Homework

- Think about your Capstone [planning phase)](https://courses.circuitstream.com/d2l/lms/dropbox/user/folder_submit_files.d2l?db=3496&grpid=0&isprv=0&bp=0&ou=9514)