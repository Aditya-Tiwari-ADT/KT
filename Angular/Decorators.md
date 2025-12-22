# TypeScript Decorators — A compact, easy-to-remember guide

Quick summary

- Decorator = special annotation that wraps a class, method, property, or parameter with extra behavior or metadata.
- Called when the class is defined (not when instances are created).
- Four types: Class, Property, Method, Parameter — remember: "CPMP" (Class, Property, Method, Parameter).

1. Basic syntax

- Decorator function signature varies by target:
  - Class: `(constructor: Function) => void | any`
  - Property: `(target: any, propertyKey: string) => void`
  - Method: `(target: any, propertyKey: string, descriptor: PropertyDescriptor) => void | PropertyDescriptor`
  - Parameter: `(target: any, propertyKey: string, parameterIndex: number) => void`
- Apply with `@Decorator` above the target.

Example

```ts
function MyDecorator(target: any) {
  console.log('decorator:', target)
}
@MyDecorator
class MyClass {}
```

2. Short examples & intent

- Class decorator — modify or seal the class

```ts
function Sealed(constructor: Function) {
  Object.seal(constructor)
  Object.seal(constructor.prototype)
}
@Sealed
class Person {}
```

- Property decorator — add metadata or make readonly

```ts
function Readonly(target: any, key: string) {
  Object.defineProperty(target, key, { writable: false })
}
class Car {
  @Readonly brand = 'Tesla'
}
```

- Method decorator — wrap/extend behavior (logging, timing)

```ts
function Log(target: any, key: string, desc: PropertyDescriptor) {
  const orig = desc.value
  desc.value = function (...args: any[]) {
    console.log(`Calling ${key}`, args)
    return orig.apply(this, args)
  }
}
class Calc {
  @Log add(a: number, b: number) {
    return a + b
  }
}
```

- Parameter decorator — attach metadata about a parameter

```ts
function LogParam(target: any, method: string, index: number) {
  console.log(`${method} param[${index}] decorated`)
}
class G {
  greet(@LogParam name: string) {}
}
```

3. One-page cheat sheet

- When run: class definition time
- Order when stacked:
  - For multiple decorators on the same declaration: evaluated bottom → top (the decorator nearest the declaration runs first).
- Typical use cases:
  - Class: metadata, sealing, mixins
  - Property: validation, readonly, reflect metadata
  - Method: logging, retry, memoize, timing
  - Parameter: DI metadata, validation hints

4. Angular quick map (most common)

- @Component — class decorator
- @Input / @Output — property decorators
- @HostListener — method decorator
- Angular uses Reflect metadata and many combined decorators

5. Memory tricks

- CPMP = Class, Property, Method, Parameter (top → bottom in declaration order).
- "Definition time" = decorators run once when class is defined.
- "Bottom-to-top evaluation" = read decorators from nearest to farthest (closest decorator executed first).

Keep this file as your cheatsheet — short examples + CPMP + rules are enough to recall details quickly.
