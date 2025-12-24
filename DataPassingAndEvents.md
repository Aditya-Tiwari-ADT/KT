# Data Passing & Events (Parent ↔ Child) — Simple Guide

This shows the two main patterns in Angular: Parent → Child with @Input, and Child → Parent with @Output + EventEmitter. Short examples and a note about $event.

## 1) Parent → Child (use @Input)

- Parent passes a value to a child property.
- Child marks that property with @Input().

Parent template and class:

```ts
// parent.component.ts
@Component({
  selector: 'app-parent',
  template: `<app-child [message]="parentMessage"></app-child>`,
})
export class ParentComponent {
  parentMessage = 'Hello from Parent!'
}
```

Child:

```ts
// child.component.ts
@Component({
  selector: 'app-child',
  template: `<p>{{ message }}</p>`,
})
export class ChildComponent {
  @Input() message = ''
}
```

How it works: the parent binds [message]="parentMessage"; the child receives it via @Input and displays it.

## 2) Child → Parent (use @Output + EventEmitter)

- Child emits events (optionally with data).
- Parent listens with event binding and receives the emitted value as $event.

Child:

```ts
// child.component.ts
@Component({
  selector: 'app-child',
  template: `<button (click)="send()">Send to Parent</button>`,
})
export class ChildComponent {
  @Output() messageEvent = new EventEmitter<string>()
  send() {
    this.messageEvent.emit('Hello from Child!')
  }
}
```

Parent:

```ts
// parent.component.ts
@Component({
  selector: 'app-parent',
  template: `
    <app-child (messageEvent)="receive($event)"></app-child>
    <p>From child: {{ childMsg }}</p>
  `,
})
export class ParentComponent {
  childMsg = ''
  receive(msg: string) {
    this.childMsg = msg
  }
}
```

How it works: child calls emit(...); parent handles it with (messageEvent)="receive($event)". $event holds the emitted value.

## Quick note: Why $event?

- $event is the template variable Angular provides for the event payload (DOM event or data emitted). Use it in templates to access the emitted value passed to the handler.

Summary:

- Use @Input() to accept data from parent.
- Use @Output() + EventEmitter to send data from child to parent; parent reads it via $event.
