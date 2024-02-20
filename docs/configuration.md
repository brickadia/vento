---
order: 2
---

# Configuration

## Options

Pass an options object to the `vento()` function to customize Vento.

```js
// Example with the default options:
const env = vento({
  dataVarname: "it",
  useWith: true,
  includes: Deno.cwd(),
  autoescape: false,
  cache: true,
});
```

### dataVarname

Vento provides the `it` variable with all data available in the template. For
example:

```vento
{{ it.title }}
```

The `dataVarname` option allows changing the name of this variable.

```js
const env = vento({
  dataVarname: "global",
});
```

Now you can use the `global` variable:

```vento
{{ global.title }}
```

### useWith

Vento use
[`with` statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/with)
to convert the variables from `it` to global. For example, instead of
`{{ it.title }}` you can simply write `{{ title }}`.

Note that if the variable doesn't exist, the error _(ReferenceError: title is
not defined)_ is thrown.

You can disable the `with` behavior by setting this option to `false`:

```js
const env = vento({
  useWith: false,
});
```

### autoescape

Set `true` to automatically escape printed variables:

```js
const env = vento({
  autoescape: true,
});

const result = env.runString("{{ title }}", {
  title: "<h1>Hello, world!</h1>",
});
// &lt;h1&gt;Hello, world!&lt;/h1&gt;

// Like in Nunjucks, you can use the `safe` filter for trusted content:
const result = env.runString("{{ title |> safe }}", {
  title: "<h1>Hello world</h1>",
});
// <h1>Hello world</h1>
```

### includes

The path of the directory that Vento will use to look for includes templates.

### cache

Set `false` to ignore the cache, and re-compile the template when loading.

```js
const env = vento({
  cache: false, // default is true
});
```

## Filters

Filters are custom functions to transform the content.

For example, let's create a function to make text italic:

```ts
function italic(text: string) {
  return `<em>${text}</em>`;
}
```

And we can register that with Vento:

```ts
env.filters.italic = italic;
```

Now you can use this filter anywhere:

```vento
<p>Welcome to {{ title |> italic }}</p>
```
