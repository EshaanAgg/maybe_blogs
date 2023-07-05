# Svelte

## Introduction

### Basic Syntax and Reactivity

- **Shorthand atributes** can be used if the name of the attribute and the variable storing the same is same.
  For example, `<image src />` is equivalent to `<image src={src} />`.
- The **styles are scoped to the component** by default in Svelte.
- It offers a `@html` tag that can be used to render the HTML into the component directly.

```html
<script>
	let htmlContent = "<h1?> Hello World </h1>";
</script>

<p>{@html htmlContent}</p>
```

This data is NOT santised before rendering.

- Computed properties are declared with the help of the `$` operator.

```js
let count = 1;
$: double = count * 2;
$: console.log("This statement will be logged every time count changes.");
$ if (count > 10){
    console.log("For multiline uses.");
    console.log("With or without the if, the bracket notation can be used.")
}
```

- Much like React, methods that change the value inplace (like `Array.push` or `Array.splice`) do not cause DOM updates.

  Using `arr = arr` is not an "idiomatic" way to trigger a re-render, but is a more memory friendly as you are mutating the object instead of copying it (in destructing).

- As the Svelte compiler works on assignment, assigments to objects (`foo.bar = 1`) and arrays (`foo[2] = bar`) cause updates.

### Props

- Use the `export` keyword to define that the variable is a prop. You can also provide it with a default value.

```js
export let prop1 = "Default value";
```

- Spread operator can also be used to send multiple values from an object to a component which expects the props of the same name.
- If you want to reference all the props that were passed to a component, including the ones that were not defined by the `export let` keywords, you can access them using `$$props`.

  **Caution:** The use of the same is difficult to optimise, and thus usually not recomended. But it may help you to solve a problem or two.

### Logic Blocks in HTML

#### Conditionals

```js
{#if porridge.temperature > 100}
    <p>Too hot!</p>
{:else if 80 > porridge.temperature}
    <p>Too cold!</p>
{:else}
    <p>Just right!</p>
{/if}
```

#### Loops

```js
{#each items as item (item.id)}
    <li> {item.name} x {item.qty} </li>
{/each}

{#each items as item, i (item.id)}
  <li> {i + 1}: {item.name} x {item.qty} </li>
{/each}
```

- You can use the `as` keyword to get the identfier that you want to refer to the variable with, and an additonal index as well. You can use `()` to provide a key to the loop indexes as well. Destructuring and rest pattern (`{id, ...rest}`) can be used freely.
- `each` block can also have an `else` block which will be rendered if the list is empty.
- Since Svelte 4 it is possible to iterate over iterables like Map or Set. Iterables need to be finite and static (they shouldn't change while being iterated over). Under the hood, they are transformed to an array using Array.from before being passed off to rendering. These might not be so great for performance though.

#### Promises

```js
{#await promise}
  <p>Waiting for the promise to resolve...</p>
{:then value}
  <p>The value of the resolved promise is {value}</p>
{:catch error}
  <p>Something went wrong: {error.message}</p>
{/await}
```

- Any of these blocks can be omitted or combined in the `await` declaration block itself.
- Only the most recent promise is considered, you don't need to worry about race conditions.

#### Keys

- Key blocks destroy and recreate their contents when the value of an expression changes.

```js
{#key value}
    <Component />
    <div transition:fade>{value}</div>
{/key}
```
