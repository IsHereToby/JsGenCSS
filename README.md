# JsGenCSS

Create CSS from JavaScript or TypeScript objects. Instead of writing CSS by hand, you define your styles as objects in your code, and this tool converts them into CSS.

## Objects to CSS Using "mod.ts"

```ts
import { object2css } from "./mod.ts";
// const obj = ...
console.log(object2css(obj)); // Convert obj to css and display to console
```

## Utilizing Deno's "generate.ts" Script

```sh
# General usage (w/ watch)
deno run --allow-read --allow-write generate.ts [INPUT_FILE] [OUTPUT_FILE] --watch

# Console output only
deno run --allow-read generate.ts [INPUT_FILE]
```

## Demo

Input:

```ts
import { array2object, object2css } from "./mod.ts";

console.log(
  object2css({
    "html, body": {
      // Simple selector
      margin: 0, // Simple property
      padding: 0,
    },
    ...array2object(["left", "right"], (k) => ({
      // "array2object" helper
      [`.text-${k}`]: {
        textAlign: k,
      },
    })),
    ".main": {
      margin: `${1 / 2}rem`, // Computed property
      ".test": {
        // Will create a ".main .test" selector
        color: "#f55",
        "&.center": {
          // The character '&' allows you to combine with the parent selector ".main .test.center"
          textAlign: "center",
        },
      },
    },
  })
);
```

Output (formatted):

```css
html,
body {
  margin: 0;
  padding: 0;
}

.text-left {
  text-align: left;
}

.text-right {
  text-align: right;
}

.main {
  margin: 0.5rem;
}

.main .test {
  color: #f55;
}

.main .test.center {
  text-align: center;
}
```
