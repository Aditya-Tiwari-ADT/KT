# Angular Signals

Angular Signals is a system that granularly tracks how and where your state is used throughout an application, allowing the framework to optimize rendering updates.

## What are signals?

A signal is a wrapper around a value that notifies interested consumers when that value changes. Signals can contain any value, from primitives to complex data structures. A signal's value is always read through a getter function, which allows Angular to track where the signal is used. Signals may be writable or read-only.

## Writable signals

Writable signals provide an API for updating their values directly. Create a writable signal with an initial value:

```ts
const count = signal(0)

// Signals are getter functions — calling them reads their value.
console.log('The count is: ' + count())
```

To change a writable signal's value you can call `.set()`:

```ts
count.set(3)
```

or use `.update()` to compute a new value from the previous one:

```ts
// Increment the count by 1.
count.update((value) => value + 1)
```

Writable signals have the type `WritableSignal`.

## Computed signals

A computed signal derives its value from other signals. Define one using `computed` with a derivation function:

```ts
const count: WritableSignal<number> = signal(0)
const doubleCount: Signal<number> = computed(() => count() * 2)
```

`doubleCount` depends on `count`. When `count` updates, anything depending on `count` or `doubleCount` will update as well.

Computed signals are lazily evaluated and memoized: the derivation function only runs the first time the computed signal is read. The result is cached and returned on subsequent reads until a dependency changes. When a dependency changes, the cached value is invalidated and recalculated on the next read. This makes computed signals safe for expensive derivations (for example, filtering large arrays).

Computed signals are not writable: attempting to set one is a compile error:

```ts
doubleCount.set(3) // Error: doubleCount is not a WritableSignal
```
