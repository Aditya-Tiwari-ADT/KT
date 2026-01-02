# NgModule (Angular)

An NgModule organizes and manages an Angular application by grouping related components, directives, pipes, and services. It enables modularity, encapsulation, and features such as lazy loading and bootstrapping. With modern Angular (14+ and improved in later versions), standalone components reduce the need for NgModules in many cases, but NgModules are still useful for certain scenarios.

## Overview

- Purpose: group related code, manage DI scope, provide imports/exports, and define the app bootstrap.
- Typical module metadata: `declarations`, `imports`, `providers`, `exports`, `bootstrap`.

## Key Responsibilities

1. Organize application code

   - Feature modules (e.g., `UserModule`, `AdminModule`)
   - Shared modules (e.g., `SharedModule` for reusable components, directives, pipes)
   - Core module for singleton services (loaded once)

2. Declare components, directives, and pipes
   - Use `declarations` to register components, directives, and pipes that belong to the module.

```ts
@NgModule({
  declarations: [HomeComponent],
})
export class HomeModule {}
```

3. Provide services (providers)
   - Use `providers` to register services at module level. Services provided in a module are available to that module's injector (or the root injector if registered in the root module).

```ts
@NgModule({
  providers: [AuthService],
})
export class AuthModule {}
```

4. Import other modules
   - Use `imports` to bring in other Angular or feature modules so you can use their exported directives/components/pipes.

```ts
@NgModule({
  imports: [CommonModule, FormsModule],
})
export class UserModule {}
```

5. Export functionality
   - Use `exports` to make declarations available to other modules that import this module.

```ts
@NgModule({
  declarations: [SharedButtonComponent],
  exports: [SharedButtonComponent],
})
export class SharedModule {}
```

6. Bootstrap the application
   - The root module (usually `AppModule`) lists the root component in `bootstrap`. Angular renders the bootstrapped component first.

```ts
@NgModule({
  declarations: [AppComponent, HomeComponent],
  imports: [BrowserModule],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

7. Lazy loading
   - Lazy loading defers loading of feature modules until required, improving initial load time.

Example (route-based lazy loading of a module):

```ts
const routes: Routes = [
  { path: 'user', loadChildren: () => import('./user/user.module').then((m) => m.UserModule) },
]
```

## Example: Basic NgModule

```ts
// home.module.ts
import { NgModule } from '@angular/core'
import { CommonModule } from '@angular/common'
import { HomeComponent } from './home/home.component'
import { AuthService } from './auth.service'

@NgModule({
  declarations: [HomeComponent],
  imports: [CommonModule],
  providers: [AuthService],
  exports: [HomeComponent],
})
export class HomeModule {}
```

## Bootstrapping flow (how Angular starts your app)

1. main.ts is the entry point:

```ts
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic'
import { AppModule } from './app/app.module'

platformBrowserDynamic()
  .bootstrapModule(AppModule)
  .catch((err) => console.error(err))
```

2. Angular bootstraps `AppModule`. It processes `@NgModule` metadata and sees `bootstrap: [AppComponent]`.

3. Angular creates an instance of `AppComponent` and renders it into the DOM element matching its selector (usually `<app-root>` in `index.html`).

4. Angular parses `AppComponent`'s template and instantiates any referenced components (e.g., `<app-home>`), resolving them from the module declarations or imports.

Note: Only the root component(s) listed in `bootstrap` are instantiated directly by the runtime. Feature components are instantiated when referenced in templates or via routing.

## Standalone components (Angular 14+)

- A standalone component can be used without declaring it in an NgModule.
- Mark with `standalone: true` and list any dependencies in `imports`.

Example:

```ts
// home.component.ts
import { Component } from '@angular/core'
import { CommonModule } from '@angular/common'

@Component({
  selector: 'app-home',
  standalone: true,
  imports: [CommonModule],
  template: `<h1>Welcome to the Home Page!</h1>`,
})
export class HomeComponent {}
```

Using a standalone root component (no AppModule required):

```ts
// main.ts
import { bootstrapApplication } from '@angular/platform-browser'
import { AppComponent } from './app/app.component' // standalone root

bootstrapApplication(AppComponent).catch((err) => console.error(err))
```

Or mixing: you can bootstrap a standalone `AppComponent` that imports other standalone components.

Routing with standalone components:

```ts
const routes: Routes = [
  { path: 'home', component: () => import('./home.component').then((m) => m.HomeComponent) },
]
```

## When to use NgModule vs standalone components

Use standalone components when:

- You want less boilerplate and a more component-centric approach.
- Building small to medium-sized features or new apps.

Keep using NgModules when:

- You rely on advanced module-level features (some complex DI scenarios, certain lazy-loading or preloading strategies).
- You need to group many related declarations and exports for large apps.

Angular supports both approaches; choose what fits your app structure and team workflow.

## Common misunderstandings

- declarations vs imports:

  - `declarations`: components, directives, and pipes that belong to this module.
  - `imports`: other modules whose exported declarations are needed in component templates of this module.

- bootstrap vs declarations:
  - `bootstrap` contains the root component(s) the runtime instantiates at application start.
  - A bootstrapped component must also be declared (or be standalone) so Angular knows about it.

## Summary

- NgModules organize app structure, control DI scope, enable lazy loading, and define bootstrap components.
- Standalone components simplify development and can reduce the need for NgModules.
- Both patterns are supported; use NgModules for complex module-level needs and standalone components for simpler, component-centric designs.
- Always declare components (or mark them standalone) and import the modules that provide the directives used in templates (e.g., `CommonModule`, `FormsModule`).
