### 3️⃣ Side-by-side comparison

| Feature                   | subscribe() | async pipe |
| ------------------------- | ----------: | :--------- |
| Used in                   |  TypeScript | HTML       |
| Unsubscribe needed        |         Yes | No         |
| Triggers change detection |      Manual | Automatic  |
| Side effects allowed      |         Yes | No         |
| Boilerplate               |        More | Less       |
| Best for                  |       Logic | UI         |

### 4️⃣ When to use async pipe ✅ (recommended)

Use async when:

- Displaying data in the template
- Working with HTTP responses
- Binding Observables to UI
- Avoiding memory leaks

Example:

```html
<div *ngIf="users$ | async as users">{{ users.length }}</div>
```

### 5️⃣ When to use subscribe() ✅

Use subscribe() when:

- You need side effects
- You must update non-template state
- Trigger routing, alerts, logging
- Combine multiple streams manually

Example:

```ts
this.authService.user$.subscribe((user) => {
  if (!user) {
    this.router.navigate(['/login'])
  }
})
```
