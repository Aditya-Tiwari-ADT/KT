# Angular Pipes

Pipes transform values in templates (or in TypeScript) using the pipe operator `|`. Arguments are passed with `:`.

Usage

- Template: `{{ value | pipeName:arg1:arg2 }}`
- Common built-in pipes: `uppercase`, `lowercase`, `date`, `percent`, `currency`, `json`, `async`

Examples

```html
<p>{{ currDate | date }}</p>
<p>{{ currDate | date:'fullDate' | uppercase }}</p>
```

Date format shortcuts

- `short`, `medium`, `long`, `full`
- Custom patterns: e.g. `h:mm:ss a`

Currency

```html
<!-- currencyCode, display, digitsInfo (minIntegerDigits.minFraction-maxFraction) -->
<p>{{ amount | currency:'EUR':'symbol':'1.1-4' }}</p>
```

In `1.1-4`: first `1` is minimum integer digits; `1-4` are min and max fraction digits.

Percent

```html
<p>{{ 1.12 | percent }}</p>
<!-- -> "112%" -->
```

Custom pipe (Append)

- Template usage:

```html
<p>{{ value | append:' string to add' }}</p>
```

- Generate with Angular CLI:

```
ng generate pipe pipes/append
```

- Example implementation:

```ts
import { Pipe, PipeTransform } from '@angular/core'

@Pipe({
  name: 'append',
  standalone: true, // omit if you declare it in an NgModule
})
export class AppendPipe implements PipeTransform {
  transform(value: string | null, appendText: string): string | null {
    if (value == null) return value
    return value + appendText
  }
}
```

If the pipe is `standalone: true`, add it to the component `imports`. Otherwise declare it in an NgModule's `declarations`.

Async and JSON pipes

- JSON pipe: transforms an object to a JSON string for display:

```html
<p>{{ user | json }}</p>
```

Example object:

```json
{
  "fname": "Ram",
  "lname": "Mohan",
  "posts": {
    "title": "Travelling",
    "updatedAt": 121212
  }
}
```

- Async pipe: subscribes to an Observable or Promise and returns the latest value. It handles subscription management automatically (subscribe/unsubscribe).

Simple async example in a template:

```html
<p>{{ user$ | async }}</p>
```

Service + component example (TypeScript):

```ts
// service
import { Injectable } from '@angular/core'
import { of, Observable } from 'rxjs'

export interface User {
  fname: string
  lname: string
}

@Injectable({ providedIn: 'root' })
export class ApiService {
  getUser(): Observable<User> {
    return of({ fname: 'Ram', lname: 'Mohan' })
  }
}

// component
export class ExampleComponent {
  user$ = this.apiService.getUser()
  constructor(private apiService: ApiService) {}
}
```

Template using async with `as` syntax:

```html
<li *ngIf="user$ | async as user">
  fname: {{ user.fname }}<br />
  lname: {{ user.lname }}
</li>
```

```html
<p>{{ user$ | async }}</p>
```

- The async pipe automatically subscribes to an Observable or Promise and unwraps (returns) the latest emitted or resolved value in the template.
- It also manages the subscription lifecycle for you (automatically unsubscribes when the view is destroyed).
- Use it with the `as` syntax to capture the unwrapped value into a template variable (e.g., `*ngIf="user$ | async as user"`).
