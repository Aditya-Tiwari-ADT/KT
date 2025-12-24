# RxJS in Angular 17 — Comprehensive Notes

RxJS (Reactive Extensions for JavaScript) is a library for reactive programming using Observables. It’s the backbone of asynchronous operations in Angular.

## 1. What is RxJS?

- Not exclusive to Angular, but integrated by default.
- Enables reactive code: applications react to events or data changes over time.
- Common uses:
  - Handle asynchronous events: HTTP requests, clicks, timers.
  - Stream data that arrives over time.
  - Compose and transform streams with operators.

## 2. Why RxJS is important in Angular

- HttpClient returns Observables by default:

```typescript
this.http.get('https://api.example.com/data').subscribe((data) => console.log(data))
```

- Reactive Forms expose `valueChanges` as an Observable:

```typescript
this.myForm.get('search')!.valueChanges.subscribe((value) => console.log(value))
```

- Routing: use `ActivatedRoute.params` reactively.
- State management (NgRx/NGXS) relies on RxJS.
- Component communication: `Subject` / `BehaviorSubject` to share data.
- Templates: `async` pipe handles subscriptions automatically:

```html
<div *ngIf="user$ | async as user">{{ user.name }}</div>
```

## 3. Observables vs Promises

| Feature      | Promise                  | Observable                              |
| ------------ | ------------------------ | --------------------------------------- |
| Nature       | Single value             | Multiple values over time               |
| Execution    | Eager (runs immediately) | Lazy (runs on subscribe)                |
| Cancellation | Cannot cancel            | Can cancel via `unsubscribe()`          |
| Composition  | `.then()` / `.catch()`   | Operators: `map`, `filter`, `switchMap` |

Note: Unsubscribe from Observables if not using the `async` pipe to avoid memory leaks.

## 4. Creating and Subscribing to Observables

a) Using `new Observable()`:

```typescript
import { Observable } from 'rxjs'

const myObservable$ = new Observable<string>((subscriber) => {
  subscriber.next('Demo text')
  subscriber.complete()
})

myObservable$.subscribe({
  next: (value) => console.log(value),
  complete: () => console.log('Completed'),
})
```

b) Using `of()` — emits the whole value once:

```typescript
import { of } from 'rxjs'

const cityList$ = of(['Delhi', 'Mumbai', 'Chennai'])
cityList$.subscribe((cities) => console.log(cities)) // Array once
```

c) Using `from()` — emits each iterable item individually:

```typescript
import { from } from 'rxjs'

const cityList$ = from(['Delhi', 'Mumbai', 'Chennai'])
cityList$.subscribe((city) => console.log(city)) // Delhi, Mumbai, Chennai
```

d) Using `interval()` & `timer()`:

- `interval(1000)`: emits sequential numbers every 1 second indefinitely.
- `timer(5000)`: emits once after 5 seconds.

## 5. RxJS Best Practices in Angular

- Naming: append `$` to observable variables (e.g., `user$`).
- Prefer the `async` pipe to manage subscriptions.
- Use operators (`map`, `filter`, `tap`, `switchMap`) instead of nesting logic inside `.subscribe()`.
- Prefer declarative streams and keep side-effects inside `tap`.
- In Angular 17+, consider combining RxJS with Signals for optimal performance.

## 6. Integrating RxJS with Angular 17 Signals

- Convert Observables to Signals using `toSignal()`:

```typescript
import { toSignal } from '@angular/core/rxjs-interop'

const data$ = this.http.get('/api/data')
const dataSignal = toSignal(data$)

// Usage in component
console.log(dataSignal()) // Returns the current value (or `undefined` until emitted)
```

## Summary

- RxJS enables reactive programming by treating data as streams.
- Core constructs: Observables, Subjects, Operators.
- Angular 17 improves interoperability with Signals (`toSignal`) for enhanced patterns and performance.

---

## pipe()

`pipe` combines multiple RxJS operators and applies them to an Observable. Operators inside `pipe` don’t run until you subscribe.

Syntax:

```typescript
observable.pipe(operator1(), operator2(), operator3())
```

Example:

```typescript
import { of } from 'rxjs'
import { map, filter } from 'rxjs/operators'

of(1, 2, 3, 4, 5)
  .pipe(
    filter((x) => x % 2 === 0), // keep even numbers
    map((x) => x * 10), // multiply by 10
  )
  .subscribe(console.log) // Output: 20, 40
```

Key points:

- Transforms, filters, combines, or handles errors.
- Keeps code modular and readable.

## next()

`next` is a method on `Subject` or `Subscriber` used to emit a value to observers.

Example with Subject:

```typescript
import { Subject } from 'rxjs'

const subject = new Subject<number>()

subject.subscribe((v) => console.log('Observer 1:', v))
subject.subscribe((v) => console.log('Observer 2:', v))

subject.next(10)
subject.next(20)
// Observer 1: 10
// Observer 2: 10
// Observer 1: 20
// Observer 2: 20
```

Example inside an Observable:

```typescript
import { Observable } from 'rxjs'

const obs = new Observable<number>((subscriber) => {
  subscriber.next(1)
  subscriber.next(2)
  subscriber.complete()
})

obs.subscribe(console.log) // Output: 1, 2
```

Quick comparison:

- pipe: chains operators to transform/filter/handle values (processing).
- next: emits a value to subscribers (pushing values into a stream).
