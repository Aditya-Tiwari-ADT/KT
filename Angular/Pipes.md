# Angular Pipes

Pipes transform values in templates (or in TS) using the pipe operator `|`. Arguments are passed with `:`.

- Usage: `{{ value | pipeName:arg1:arg2 }}`
- Common built-in pipes: `uppercase`, `lowercase`, `date`, `percent`, `currency`

Examples:

```html
<p>{{ currDate | date }}</p>
<p>{{ currDate | date:'fullDate' | uppercase }}</p>
```

Date format shortcuts: `'short'`, `'medium'`, `'long'`, `'full'`, and patterns like `'h:mm:ss'`.

Currency:

```html
<!-- currencyCode, display, digitsInfo (minIntegerDigits.minFraction-maxFraction) -->
<p>{{ amount | currency:'EUR':'symbol':'1.1-4' }}</p>
```

In `1.1-4`, the first `1` is the minimum integer digits, and `1-4` is the min and max fraction digits.

Percent:

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
  standalone: true, // omit if you will declare it in an NgModule
})
export class AppendPipe implements PipeTransform {
  transform(value: string | null, appendText: string): string | null {
    if (value == null) return value
    return value + appendText
  }
}
```

If the pipe is `standalone: true`, add it to the component `imports`. Otherwise declare it in an NgModule's `declarations`.
