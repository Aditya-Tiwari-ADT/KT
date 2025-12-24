# Organizing the Application into Feature Modules

Feature modules are self-contained packages that group related components, directives, pipes, services, and routing. This improves maintainability, enables lazy loading, and keeps responsibilities separated.

Example — E-commerce project structure:

```text
/src
├── /app
│   ├── /core                # Core services & singletons (auth, logging)
│   ├── /shared              # Shared components, pipes, directives
│   ├── /user                # User module (registration, login, profile)
│   ├── /product             # Product module (listing, details, reviews)
│   ├── /cart                # Cart module (add to cart, review, checkout)
│   ├── /order               # Order module (order history, tracking)
│   ├── app.module.ts        # Root module
│   └── app-routing.module.ts# Root routing module
```

Guidelines:

- Keep feature modules focused on a single domain or capability.
- Put singleton services in `core` and reusable UI in `shared`.
- Prefer lazy loading for large or rarely used feature modules.
- Use a root routing module (`app-routing.module.ts`) to compose feature routes.
- Keep each module's public API minimal: export only components/directives/pipes needed by other modules.
- Structure each feature with clear subfolders: `components/`, `services/`, `models/`, and `routing/`.

Services contain business logic. For example, the product module's services handle fetching product data and related operations.

## Step 3: Lazy Loading the Modules

To optimize performance, lazy-load feature modules when a user navigates to their route. This reduces the initial bundle size and speeds up application load time.

Example AppRoutingModule:

```ts
// app-routing.module.ts
import { NgModule } from '@angular/core'
import { RouterModule, Routes } from '@angular/router'

const routes: Routes = [
  { path: 'user', loadChildren: () => import('./user/user.module').then((m) => m.UserModule) },
  {
    path: 'products',
    loadChildren: () => import('./product/product.module').then((m) => m.ProductModule),
  },
  { path: 'cart', loadChildren: () => import('./cart/cart.module').then((m) => m.CartModule) },
  { path: 'orders', loadChildren: () => import('./order/order.module').then((m) => m.OrderModule) },
  { path: '', redirectTo: '/products', pathMatch: 'full' },
]

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
})
### Pipes — Transforming Data
```

- Pipes transform displayed data in templates.
- Define a pipe with the `@Pipe` decorator and implement `PipeTransform`:Pipes are used for transformation of data it is defined using @Pipe and transform method is used it can be used inside templates or inside observables

```ts
import { Pipe, PipeTransform } from '@angular/core'

@Pipe({ name: 'myPipe' })
export class MyPipe implements PipeTransform {
  transform(value: any, ...args: any[]): any {
    // transform logic
    return /* transformed value */
  }
}
```

- Use a pipe in a template with the pipe operator `|`:

```html
<p>{{ abc.title | uppercase }}</p>
```

- The example above uses Angular's built-in `uppercase` pipe to render `abc.title` in uppercase.
- Pipes can also be used in code by injecting the pipe and calling its `transform` method, or by applying equivalent transformation logic inside RxJS operators (e.g., `map`) when working with observables.
- Prefer pure pipes for stateless transformations; use impure pipes only when necessary for changing input references.
- Ensure pipes used in templates are declared in a module imported by the component (e.g., `CommonModule` for built-in pipes).
<p>{{abc.title | uppercase}}<p>

in the above example uppercase is a pipe
