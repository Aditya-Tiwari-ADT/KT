# Data Binding — style, attribute, class, event, and two-way binding

## Passing values from TypeScript to HTML

Data defined in the component class are bound to the template.

TypeScript (component):

```ts
export class AppComponent {
  title: string = 'Introduction'
  isDisabled: boolean = true
  bgColor: string = 'red'
}
```

HTML:

```html
<p>hello {{ title }}</p>
<button [style.backgroundColor]="bgColor" [disabled]="isDisabled"></button>
```

## Class binding (adding classes)

Apply a class to an element conditionally.

TypeScript:

```ts
export class AppComponent {
  redText: string = 'abcd'
}
```

HTML:

```html
<h2 [class.textColor]="redText === 'abcd'">Some heading</h2>
```

The `textColor` class can be defined in a global SCSS/CSS file.

## Event binding

Syntax for event binding: `(eventName)="handler()"`. Syntax for attribute/property binding: `[property]="expression"`.

Example (click event):

```html
<button (click)="increment()">Count = {{ count }}</button>
```

Passing value from HTML to TypeScript using events:

```html
<input (input)="onInputValueChange($event)" />
```

In the handler you can read the value via `event.target.value`.

## Two-way binding (ngModel)

Two-way binding synchronizes data between HTML and TypeScript: HTML ⇄ TS.

Basic syntax:
HTML:

```html
<input [(ngModel)]="username" name="username" />
```

TypeScript:

```ts
username = ''
```

As the user types, the `username` property updates. Changing `username` in TS updates the input.

What `[(ngModel)]` means:

```html
<input [ngModel]="username" (ngModelChange)="username = $event" />
```

- `[ngModel]` → sends value from TS to HTML
- `(ngModelChange)` → sends value from HTML to TS

Note: To use `ngModel`, import `FormsModule` in the Angular module and give the control a `name` if it's inside a form:

```ts
import { FormsModule } from '@angular/forms'
@NgModule({
  imports: [
    FormsModule,
    // ...
  ],
})
export class AppModule {}
```
