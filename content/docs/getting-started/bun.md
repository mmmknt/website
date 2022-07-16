# Bun

Bun is another JavaScript runtime. It's not Node.js or Deno. Bun includes a trans compiler, so we can write the code with TypeScript.

Hono also works on Bun. Some middleware does not work with Bun, so please keep it in mind.

## 1. Install

### `bun` command

To install `bun` command, follow the instruction in [the official web site](https://bun.sh).

### `bun install`

Make the project directory, move into it, and run `bun install` command to install Hono from npm registry.

```
bun install hono
```

## 2. Hello World

"Hello World" script is below. Almost the same as writing on other platforms.

```ts
import { Hono } from "hono"

const app = new Hono()
const app = app.get("/", (c) => c.text('Hello! Hono!'))

export default {
  port: 3000,
  fetch: app.fetch,
}
```

## 3. Run

Run the command.

```ts
bun run index.ts
```

Then, access `http://localhost:3000` in your browser.

## `bun create`

You can also make a skelton project of Hono by the command below:

```
bun create hono ./my-app
```

## Unsupported Middleware

Currently, Bun does not support some function for working the application correct with Hono.

* **JWT middleware** does not work on Bun yet. **DO NOT USE** *JWT* middleware on Bun.
* "JSX Pragma" is not supported on Bun yet. So, you don't have to use **JSX middleware** on Bun. If you want to write JSX, use "React" instead of *JSX* middleware.

## Advanced

### Testing

You can use `node:assert` for testing on Bun.

```ts
import assert from 'node:assert'
import { Hono } from 'hono'

const app = new Hono()
app.get('/', (c) => c.text('Please test me'))

const req = new Request('http://localhost/')
const res = await app.request(req)
assert.strictEqual(res.status, 200)
```

### With React

React also works on Bun.
With `renderToString()` or `renderToReadableStream()` in `react-dom/server'`, you can write HTML with JSX syntax for Server Side Rendering.

First, install React libraries.

```
bun add react react-dom
bun add -d @types/react @types/react-dom
```

And, write `index.tsx`:

```tsx
import { Hono } from 'hono'
import React from 'react'
import { renderToString } from 'react-dom/server'

const app = new Hono()

app.get('/', (c) => {
  const hello = (
    <html>
      <body>
        <h1>Hello! React!</h1>
      </body>
    </html>
  )
  return c.html(renderToString(hello))
})

export default app
```