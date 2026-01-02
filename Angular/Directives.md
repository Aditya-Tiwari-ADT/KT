# Directives

## Built-in directives

### ngClass and ngStyle

Use attribute bindings to apply classes and styles.

```html
<div [ngClass]="classString" [ngStyle]="styleString"></div>
```

```ts
import { NgClass, NgStyle } from '@angular/common'
```

## Structural directives

Common structural directives: `NgIf`, `NgForOf`, `NgSwitch`, `NgSwitchCase`, `NgSwitchDefault`.

### ngIf

Conditional rendering with `*ngIf`.

```html
<!-- simple condition -->
<div *ngIf="isLoggedIn && check">...</div>

<!-- if / else using ng-template -->
<div *ngIf="isLoggedIn; else loggedOut">Welcome back!</div>

<ng-template #loggedOut>
  <div>Please log in</div>
</ng-template>
```

```ts
isLoggedIn = true
```

Chaining if / else-if by nesting `ng-template`:

```html
<div *ngIf="status === 'loading'; else content"></div>

<ng-template #content>
  <div *ngIf="status === 'error'; else success">Error occurred</div>
</ng-template>

<ng-template #success> Data loaded successfully </ng-template>
```

### ngFor

```ts
names = ['rohan', 'mohan', 'chohan']
```

```html
<ul>
  <li *ngFor="let name of names; let i = index">{{ name }}</li>
</ul>
```

### ngSwitch

```html
<div [ngSwitch]="grade">
  <p *ngSwitchCase="'A'">Excellent</p>
  <p *ngSwitchCase="'B'">Good</p>
  <p *ngSwitchCase="'C'">Bad</p>
  <p *ngSwitchDefault>All the best</p>
</div>
```

## New control-flow syntax (Angular 17)

New compact directives: `@if`, `@for`, `@switch` (syntax examples).

@if / @else:

```html
@if (a > b) {
<p>{{ a }} is greater than {{ b }}</p>
} @else if (a === b) {
<p>They are equal</p>
} @else {
<p>{{ b }} is greater</p>
}
```

@for:

```html
@for (let item of items; track item.id; let i = $index; let total = $count) { {{ item.title }} - {{
i }} }
```

Local iteration variables: `$first`, `$last`, `$odd`, `$even`, `$index`, `$count`.

@switch / @case:

```html
@switch (grade) { @case ('A') {
<p>Excellent</p>
} @default {
<p>Default case</p>
} }
```
