# Services and Dependency Injection (Angular)

## Overview

- Service: reusable, non-UI code shared among components.
- Typical tasks: API calls, data fetching, business logic.
- Services are provided via Dependency Injection (DI). When provided in the root injector they are singletons (one shared instance across the app).

## Generate a service

- CLI: `ng g s services/data` — creates `data.service.ts`.

## Key points

- Use `@Injectable()` so Angular's DI can create and inject the service.
- `providedIn: 'root'` registers the service at the root injector (application-wide singleton).
- Adding a service to a component's `providers` creates a new instance scoped to that component.

## Example service (TypeScript)

```ts
import { Injectable } from '@angular/core'

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private data: string[] = ['Joe', 'Vita', 'Mike']

  getData(): string[] {
    return this.data
  }
}
```

## Using a service in a component

- Inject the service via the constructor. Do not add it to `providers` unless you want a separate instance.

```ts
import { Component, OnInit } from '@angular/core'
import { DataService } from './services/data.service'

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
})
export class AppComponent implements OnInit {
  data: string[] = []

  constructor(private dataService: DataService) {}

  ngOnInit(): void {
    this.data = this.dataService.getData()
  }
}
```

## HTTP API calls (HttpClient)

- Import `HttpClientModule` in the app module `imports` (not `providers`).

```ts
// app.module.ts
import { HttpClientModule } from '@angular/common/http'

@NgModule({
  imports: [
    BrowserModule,
    HttpClientModule,
    // ...
  ],
})
export class AppModule {}
```

- Service using `HttpClient`:

```ts
import { Injectable } from '@angular/core'
import { HttpClient } from '@angular/common/http'
import { Observable } from 'rxjs'
import { Post } from './interfaces/post.model' // example interface

@Injectable({
  providedIn: 'root',
})
export class ApiService {
  private apiUrl = 'https://jsonplaceholder.typicode.com/posts'

  constructor(private http: HttpClient) {}

  getPosts(): Observable<Post[]> {
    return this.http.get<Post[]>(this.apiUrl)
  }
}
```

- Consume the observable in a component and subscribe (typically in `ngOnInit`):

```ts
import { Component, OnInit } from '@angular/core'
import { ApiService } from './services/api.service'
import { Post } from './interfaces/post.model'

@Component({
  /* ... */
})
export class PostsComponent implements OnInit {
  posts: Post[] = []

  constructor(private apiService: ApiService) {}

  ngOnInit(): void {
    this.apiService.getPosts().subscribe({
      next: (response: Post[]) => (this.posts = response),
      error: (error: any) => console.error(error),
    })
  }
}
```

- Template example:

```html
<ul>
  <li *ngFor="let post of posts">{{ post.title }}</li>
</ul>
```

## Types and interfaces

- Generate an interface: `ng g interface interfaces/post` and declare the expected shape:

```ts
export interface Post {
  userId: number
  id: number
  title: string
  body: string
}
```

## Lifecycle hook note

- Use `ngOnInit()` (implements `OnInit`) for initialization logic that should run when the component is mounted.
