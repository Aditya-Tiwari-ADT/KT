# RxJS in Angular 17 — Comprehensive Notes

RxJS (Reactive Extensions for JavaScript) is a library for reactive programming using Observables. It is the backbone of asynchronous operations in Angular.

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
- State management (NgRx/NGXS) relies heavily on RxJS.
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

Note: Always unsubscribe from Observables if not using the `async` pipe to avoid memory leaks.

## 4. Creating and Subscribing to Observables

a) Using `new Observable()`:

```typescript
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
const cityList$ = of(['Delhi', 'Mumbai', 'Chennai'])
cityList$.subscribe((cities) => console.log(cities)) // Array once
```

c) Using `from()` — emits each iterable item individually:

```typescript
const cityList$ = from(['Delhi', 'Mumbai', 'Chennai'])
cityList$.subscribe((city) => console.log(city)) // Delhi, then Mumbai, then Chennai
```

d) Using `interval()` & `timer()`:

- `interval(1000)`: emits sequential numbers every 1 second indefinitely.
- `timer(5000)`: emits once after 5 seconds.

## 5. RxJS Best Practices in Angular

- Naming: append `$` to observable variables (e.g., `user$`).
- Prefer the `async` pipe to manage subscriptions.
- Use operators (`map`, `filter`, `tap`, `switchMap`) instead of nesting logic inside `.subscribe()`.
- In Angular 17+, consider combining RxJS with Signals for optimal performance.

## 6. Integrating RxJS with Angular 17 Signals

- Convert Observables to Signals using `toSignal()`:

```typescript
import { toSignal } from '@angular/core/rxjs-interop'

const data$ = this.http.get('/api/data')
const dataSignal = toSignal(data$)

// Usage in component
console.log(dataSignal()) // Returns the current value of the stream
```

✅ Summary

- RxJS enables reactive programming by treating data as streams.
- Core constructs: Observables, Subjects, Operators.
- Angular 17 improves interoperability with Signals (`toSignal`) for enhanced patterns and performance.
