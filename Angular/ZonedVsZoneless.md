# Zoned vs Zoneless

## What problem are we trying to solve?

When building interactive apps (clicks, timers, HTTP responses), Angular needs to know when to run change detection so the UI stays in sync. By default Angular relied on Zone.js to intercept async operations and automatically trigger change detection. That works but can be inefficient — change detection can run far more often than necessary.

---

## The Zone story

Zone.js patches browser async APIs (events, timers, promises, XHR/fetch, micro/macrotasks) and notifies Angular (e.g., via `zone.onMicrotaskEmpty`) so Angular runs change detection automatically.

### Example (zone-based)

```ts
export class UserComponent {
  userName = 'Gitesh'

  changeName() {
    this.userName = 'Jane'
  }
}
```

Template:

```html
<h1>{{ userName }}</h1>
<button (click)="changeName()">Change Name</button>
```

Flow: click → Zone.js catches event → `changeName()` runs → Zone signals → Angular runs change detection → UI updates.

---

## The performance problem

Because Zone.js catches many async events, Angular can run change detection very frequently (e.g., every 100ms for `setInterval`), wasting CPU and battery if nothing relevant changed.

Example:

```ts
ngOnInit() {
  setInterval(() => {
    this.counter++;
  }, 100);
}
```

---

## OnPush change detection

`OnPush` reduces work by telling Angular to check a component only when its `@Input()` references change or an event originates inside the component.

```ts
@Component({
  selector: 'app-user',
  template: `<h1>{{ userName }}</h1>`,
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class UserComponent {
  @Input() userName: string
}
```

This is more efficient but must be applied deliberately and can introduce subtle bugs if you rely on mutation rather than immutable updates.

---

## What is zoneless?

Zoneless is an alternative approach (improved in Angular 16+) where change detection is explicit. Instead of Zone.js deciding when to run detection, application code updates reactive primitives (signals) and Angular updates exactly what depends on them.

### Signals example

````ts
import { signal } from '@angular/core';

export class UserComponent {
  userName = signal('Gitesh');

  changeName() {
    this.userName.set('Jane');
  }
}
```What is Zoneless?
Template: new hotness: zoneless. This is a completely different approach to change detection that Angular introduced in version 16 and has been improving.
```html
<h1>{{ userName() }}</h1>Instead of Zone.js watching everything and triggering change detection constantly, the zoneless approach is more explicit. You tell Angular exactly when to run change detection.
<button (click)="changeName()">Change Name</button>
```The idea is simple: your application code explicitly notifies Angular when changes happen. You’re not relying on Zone.js to spy on everything.
Signals are functions; when a signal updates, only the consumers of that signal re-render.
How Zoneless Actually Works
--- Angular, Zone.js is still there, but it’s not making decisions about when to run change detection. Instead, Angular relies on something called “signals” and “effect” (if you’re using Angular’s reactivity system).

## Enabling zonelessHere’s a simple example:
Example using the experimental provider:
```tsimport { signal } from '@angular/core';
import { provideExperimentalZonelessChangeDetection } from '@angular/core';
export class UserComponent {
bootstrapApplication(AppComponent, {
  providers: [
    provideExperimentalZonelessChangeDetection(),changeName() {
  ],t('Jane');
});
````

Or using legacy bootstrap options:ith signals, when you update a signal value, it knows exactly which parts of your component need to update. There’s no guessing, no change detection running when it doesn’t need to.

````ts
bootstrapModule(AppModule, {In your template:
  ngZone: 'zone.js' // or 'noop' for zoneless behavior
});<h1>{{ userName() }}</h1>
```me()">Change Name</button>
functions. When the signal updates, anything that reads from it automatically re-renders.
---
Zoneless with Dependency Injection
## Key differences (summary)gular application like this:
- Zone-based: Zone.js patches async APIs and triggers change detection broadly.
- Zoneless: App explicitly updates state via signals/effects; Angular updates only the affected parts.import { provideExperimentalZonelessChangeDetection } from '@angular/core';

---bootstrapApplication(AppComponent, {

## A more realistic exampleentalZonelessChangeDetection(),

### Zone-based approach
```tsin a traditional NgModule:
@Component({
  selector: 'app-todos',import { NgZone } from '@angular/core';
  template: `
    <div *ngFor="let todo of todos">bootstrapModule(AppModule, {
      {{ todo.title }}'noop' for zoneless
    </div>
    <button (click)="addTodo()">Add</button>n you enable zoneless, you’re telling Angular “I’m going to manage when change detection runs. Stop watching everything.”
  `
})Key Differences
export class TodosComponent implements OnInit {one based versus zoneless side by side:
  todos: any[] = [];
Press enter or click to view image in full size
  constructor(private http: HttpClient) {}
A More Complex Example
  ngOnInit() {
    this.http.get('/api/todos').subscribe(data => {Let’s build something more realistic. Imagine you have a todo list with network calls:
      this.todos = data;
      // Zone.js catches this and triggers change detectionThe Zone Based Approach
    });
  }@Component({
pp-todos',
  addTodo() {or="let todo of todos">
    this.http.post('/api/todos', {}).subscribe(newTodo => {.title }}
      this.todos.push(newTodo);
      // Zone.js catches this tooddTodo()">Add</button>
    });
  }
}ort class TodosComponent implements OnInit {
```dos: any[] = [];

### Zoneless approach with signalshttp: HttpClient) {}
```ts
@Component({
  selector: 'app-todos',this.http.get('/api/todos').subscribe(data => {
  template: `ata;
    <div *ngFor="let todo of todos()">on
      {{ todo.title }}
    </div>
    <button (click)="addTodo()">Add</button>
  `,Todo() {
  changeDetection: ChangeDetectionStrategy.OnPushthis.http.post('/api/todos', {}).subscribe(newTodo => {
})sh(newTodo);
export class TodosComponent implements OnInit {
  todos = signal<any[]>([]);

  constructor(private http: HttpClient) {}
e.js intercepts the HTTP responses and tells Angular to update.
  ngOnInit() {
    this.http.get('/api/todos').subscribe(data => {
      this.todos.set(data);The Zoneless Approach with Signals
    });
  }
selector: 'app-todos',
  addTodo() {   <div *ngFor="let todo of todos()">
    this.http.post('/api/todos', {}).subscribe(newTodo => {
      this.todos.update(current => [...current, newTodo]);
    });utton>
  }
}ction: ChangeDetectionStrategy.OnPush
````

In the zoneless variant you explicitly call `set()` or `update()` on signals so Angular knows precisely what changed.rt class TodosComponent implements OnInit {

---

## Bottom line

- Zone.js makes change detection automatic but can be noisy.ngOnInit() {
- OnPush helps reduce checks but requires careful state handling. => {
- Zoneless + signals moves to explicit, fine-grained reactivity: fewer unnecessary checks and more predictable updates.this.todos.set(data);
- Choose the model that fits your app’s performance and developer ergonomics.tly update the signal
