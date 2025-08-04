---
title: "Modern Angular Refresh"
---

## Modern Angular Refresh

Note: For folks who've used Angular before, or fullstackers who haven’t touched it in a while. Goal: Get everyone productive and up to date with modern Angular practices.

---

## Angular Today

- Current Angular version: 20.1.4<!-- .element: class="fragment" -->
- Standalone APIs<!-- .element: class="fragment" -->
- Signals stable<!-- .element: class="fragment" -->
- Release cadence: major every 6 months, LTS policy<!-- .element: class="fragment" -->
  
<small class="fragment">
<a href="https://angular.dev/reference/releases#actively-supported-versions" target="_blank">Angular.dev reference</a>
</small>

Note: High-level overview, good to give people orientation on what's changed since they last looked.

---

## Angular CLI

- `ng new`
- Built-in testing, linting, formatting

```bash
ng new angular-refresh
cd angular-refresh
ng serve
```

Note: Best practices, standalone by default, configurations for all out of the box.

----

<ul>
  <li class="fragment">
    <pre>ng generate [component|service|pipe|etc]</pre>
  </li>
  <li class="fragment">
    <pre>ng g c shared/ui/button</pre>
  </li>
  <li class="fragment">
    <pre>ng g s core/services/auth</pre>
  </li>
</ul>

---

## Anatomy of an Angular App

```folder
src/
  app/
    features/
      task-list/
        task-list.component.ts
        task-item.component.ts
        task-form.component.ts
      task.service.ts
    app.component.ts
    app.routes.ts
    main.ts
```
<!-- .element: class="fragment" -->

Note: Diagram: Component Tree, Services, Routing, Modules (optional)

----

```folder
src/
  app/
    features/
      task-list/
        task-list.ts // component
        task-item.ts // component 
        task-form.ts // component 
      tasks.ts // service / repository / business logic
    app.ts // component
    app.routes.ts
    main.ts
```

Note: Modern new defaults, no more component/service in the name. Discuss?

----

### Angular Architecture Refresher

- Components = UI logic & template
- Smart components = "Wiring"
- Services = business logic & data
- Routing

Note: Go through each, from the smallest part into the more "smart" elements.

----

```ts
@Component({
  selector: 'my-app',
  template: `<h1>Hello {{ name }}</h1>`
})
export class AppComponent {
  name = 'Angular';
}
```

Note: Simplest app component.

----

## Old Inputs & Outputs

- @Input() and @Output() still alive and well
- Used for communication between parent/child

```ts
@Input() title: string = '';
@Output() clicked = new EventEmitter<void>();
```

Note: Great place for a live example: counter button emitting events to a parent.

----

# Signal based Inputs & Outputs

- input() & output()
- Used for communication between parent/child
- Signal based changes

```ts
title = input<string>(); // ReadOnlySignal<>
clicked = output(); // has a `.emit()` like EventEmitters did.
```

Note: Great place for a live example: counter button emitting events to a parent.

---

## Communication Pattern Options

<!-- TODO table: Inputs/Outputs vs Service-based vs State Management -->

- Start with Inputs/Outputs<!-- .element: class="fragment" -->
- Add services for cross-component state<!-- .element: class="fragment" -->
- Signals? Great for local component state<!-- .element: class="fragment" -->

Keep your (shared) UI components as "dumb" as possible. Services are a form of coupling!<!-- .element: class="fragment" -->

Note: Acknowledge that there are many paths, depending on app scale.

---

## Signals! The New Reactive Primitive

```ts
import { signal, computed, effect } from '@angular/core';

const count = signal(0);

effect(() => console.log('Count changed:', count()));
```

Note: Think of Signals as Angular’s new way of handling state. It’s local, reactive, and avoids the complexity of Observables.

---

## Hands-on: Build a Signal Counter
<!-- TODO: Create component with signal-based counter -->

----

```ts
@Component({
  standalone: true,
  selector: 'counter',
  template: `
    <button (click)="inc()">Clicked {{ count() }} times</button>
  `
})
export class Counter {
  count = signal(0);
  inc() { this.count.update(n => n + 1); }
}
```

Note: Super clean. No RxJS. Ask: Who prefers this over BehaviorSubject?

---

## Signals vs Observables
<!-- TODO: make comparison table -->
Feature	Signals	RxJS Observables
Pull-based	✅	❌ (push-based)
Async support	❌	✅
Built-in to Angular	✅	✅
Great for...	Local UI state	Async streams, effects

Note: Signals aren't replacing RxJS, but great for local state and reactivity.

---

## State Management in Angular

- Still many options: RxJS, NGXS, Akita, NgRx
- For many apps, Signals + Services are enough
- RxJS still great for async tasks

Note: Show a basic state service pattern with a Signal store

----

### Optional: Build a Tiny Signal-based Store
<!-- TODO: Create a simple service using signals for shared state -->

```ts
@Injectable({ providedIn: 'root' })
export class TodoStore {
  private todos = signal<string[]>([]);
  readonly count = computed(() => this.todos().length);

  add(todo: string) {
    this.todos.update(t => [...t, todo]);
  }
}
```

Note: Great bridge to modern state patterns.

---

# Summary
## What’s New & What Matters

✅ CLI for best practices
✅ Standalone components
✅ Signals = reactive state
✅ Angular is leaner, faster, more approachable

---
