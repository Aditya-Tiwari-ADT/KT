# RxJS Methods — Beginner-Friendly Guide

RxJS operators grouped by purpose with simple examples.

---

## 1. Creation operators (create Observables)

These create Observables.

- `of()` — Emits given values one by one.

```js
of(1, 2, 3).subscribe(console.log)
// 1, 2, 3
```

- `from()` — Converts an array, promise, or iterable into an Observable.

```js
from([10, 20, 30]).subscribe(console.log)
```

- `fromEvent()` — Creates an Observable from events.

```js
fromEvent(button, 'click').subscribe(() => console.log('Clicked'))
```

- `interval()` — Emits numbers at a fixed time interval.

```js
interval(1000).subscribe(console.log)
// 0, 1, 2, ...
```

---

## 2. Transformation operators (change data)

Modify values emitted by an Observable.

- `map()` — Transforms each value.

```js
of(1, 2, 3)
  .pipe(map((x) => x * 2))
  .subscribe(console.log)
// 2, 4, 6
```

- `scan()` — Like `reduce`, but emits intermediate results.

```js
of(1, 2, 3)
  .pipe(scan((acc, val) => acc + val, 0))
  .subscribe(console.log)
// 1, 3, 6
```

---

## 3. Filtering operators (control what passes)

Filter or limit values.

- `filter()` — Allows values matching a condition.

```js
of(1, 2, 3, 4)
  .pipe(filter((x) => x % 2 === 0))
  .subscribe(console.log)
// 2, 4
```

- `take()` — Takes first N values, then completes.

```js
interval(1000).pipe(take(3)).subscribe(console.log)
// 0, 1, 2
```

- `debounceTime()` — Waits before emitting the latest value.

```js
fromEvent(input, 'input').pipe(debounceTime(300)).subscribe(console.log)
```

---

## 4. Combination operators (merge streams)

Combine multiple Observables.

- `merge()` — Emits values from all sources as they arrive.

```js
merge(interval(1000), interval(1500)).subscribe(console.log)
```

- `combineLatest()` — Emits latest values from each source.

```js
combineLatest([of(1, 2), of('A', 'B')]).subscribe(console.log)
```

- `forkJoin()` — Waits for all sources to complete, then emits once.

```js
forkJoin({
  user: user$,
  posts: posts$,
}).subscribe(console.log)
```

---

## 5. Higher-order mapping (handling async inside async)

Very important for async flows.

- `switchMap()` — Cancels previous inner Observable.

```js
fromEvent(input, 'input')
  .pipe(switchMap(() => fetchData()))
  .subscribe(console.log)
```

Best for search/autocomplete.

- `mergeMap()` — Runs inner Observables in parallel.

```js
of(1, 2, 3)
  .pipe(mergeMap((id) => getUser(id)))
  .subscribe(console.log)
```

- `concatMap()` — Queues inner Observables (one after another).

```js
of(1, 2, 3)
  .pipe(concatMap((id) => saveData(id)))
  .subscribe(console.log)
```

- `exhaustMap()` — Ignores new emissions while one inner Observable is running.

```js
fromEvent(button, 'click')
  .pipe(exhaustMap(() => save()))
  .subscribe()
```

Best for submit buttons.

---

## 6. Error handling operators

- `catchError()` — Handles errors and recovers.

```js
apiCall()
  .pipe(catchError((err) => of('Error occurred')))
  .subscribe(console.log)
```

- `retry()` — Retries on error.

```js
apiCall().pipe(retry(3)).subscribe()
```

---

## 7. Utility operators

- `tap()` — Side effects for logging or debugging.

```js
of(1, 2, 3).pipe(tap(console.log)).subscribe()
```

- `finalize()` — Runs when the Observable completes or errors.

```js
apiCall()
  .pipe(finalize(() => console.log('Done')))
  .subscribe()
```

---

## 8. Multicasting & state

- `Subject` — Acts as both Observable and Observer.

```js
const subject = new Subject()
subject.subscribe(console.log)
subject.next(1)
```

- `BehaviorSubject` — Stores and emits the latest value.

```js
const bs = new BehaviorSubject(0)
bs.subscribe(console.log)
bs.next(5)
```
