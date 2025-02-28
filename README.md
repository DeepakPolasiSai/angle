# Angular Notes

## 📌 Angular Assets Configuration

### 🔹 Why Configure Assets in `angular.json`?

In Angular, assets such as images, fonts, and other static files need to be correctly mapped in the `angular.json` file. If not configured properly, these assets won't be accessible in the app.

### 🔹 Default Asset Configuration

By default, the `angular.json` file includes:

```json
"assets": [
  "src/favicon.ico",
  "src/assets"
]
```

This means:

- \`\`: The favicon is directly included.
- \`\`: Everything inside `src/assets/` is available to the app.

### 🔹 What Happens If `src/assets` is Missing?

If you remove `"src/assets"` from `angular.json`, your app won’t be able to access any assets stored inside `src/assets/`. For example, if your images are inside `src/assets/images/logo.png`, but `angular.json` does not have `"src/assets"`, the image will **not** be displayed in the app.

### 🔹 How to Fix Missing Assets Issue?

Ensure `angular.json` includes:

```json
"assets": [
  "src/favicon.ico",
  "src/assets"
]
```

Then, reference the asset in your components like:

```html
<img src="assets/images/logo.png" alt="Logo">
```

✅ **No need to include ****\`\`**** in the path.**

### 🔹 Customizing Asset Paths

If you want to store assets in a different location (e.g., `public_assets` instead of `assets`), update `angular.json`:

```json
"assets": [
  "src/favicon.ico",
  "src/public_assets"
]
```

Now, reference assets using:

```html
<img src="assets/images/logo.png" alt="Logo">
```

Even though files are inside `src/public_assets`, they are still accessible via `assets/`.

### ✅ Summary Table

| Scenario                                        | Result                                  |
| ----------------------------------------------- | --------------------------------------- |
| `"src/assets"` in `angular.json`                | ✅ Assets load properly                  |
| `"src/assets"` removed from `angular.json`      | ❌ Assets won't load                     |
| Custom asset folder (e.g., `src/public_assets`) | ✅ Works if configured in `angular.json` |
| Referencing assets with `/src/` in the path     | ❌ Incorrect                             |

---

## 📌 Angular Components

### 🔹 What is a Component?

A **component** in Angular is the fundamental building block of an Angular application. It controls a section of the UI and consists of:

- **Template (HTML)** – Defines the UI.
- **Class (TypeScript)** – Contains logic & data.
- **Styles (CSS/SCSS)** – Defines appearance.

### 🔹 Creating a Component

✅ **Using Angular CLI:**

```sh
ng generate component component-name
# or
ng g c component-name
```

This generates:

```
/src/app/
  ├── component-name/
  │   ├── component-name.component.ts
  │   ├── component-name.component.html
  │   ├── component-name.component.css
  │   └── component-name.component.spec.ts
```

### 🔹 Component Structure

A component is defined using the `@Component` decorator.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',  // 👈 Custom HTML tag
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css']
})
export class ExampleComponent {
  title = 'Hello Angular!';
}
```

### 🔹 Adding a Component to HTML

Once created, the component's **selector** can be used as a custom HTML tag in another component:

```html
<app-example></app-example>
```

### 🔹 Standalone Components (`standalone: true`)

Introduced in Angular 14, it allows components to work **without needing a module**.

```typescript
@Component({
  selector: 'app-standalone',
  standalone: true, // 👈 No need for NgModule
  template: `<h1>Hello from Standalone Component!</h1>`,
  styleUrls: ['./standalone.component.css']
})
export class StandaloneComponent { }
```

---

## 📌 Signals and Computed Properties in Angular

### 🔹 What are Signals?

Angular **signals** are a new reactive state management feature that provides fine-grained reactivity without requiring external libraries like RxJS.

### 🔹 Creating a Signal

```typescript
import { signal } from '@angular/core';

export class ExampleComponent {
  count = signal(0);
  
  increment() {
    this.count.set(this.count() + 1);
  }
}
```

### 🔹 Using Signals in Templates

```html
<p>Count: {{ count() }}</p>
<button (click)="increment()">Increase</button>
```

### 🔹 Computed Properties in Angular

Computed properties in Angular allow **derived state** from existing signals without manually updating them.

```typescript
import { signal, computed } from '@angular/core';

export class ExampleComponent {
  count = signal(0);
  doubleCount = computed(() => this.count() * 2);
  
  increment() {
    this.count.set(this.count() + 1);
  }
}
```

### 🔹 Using Computed Properties in Templates

```html
<p>Count: {{ count() }}</p>
<p>Double Count: {{ doubleCount() }}</p>
<button (click)="increment()">Increase</button>
```

✅ **Computed properties automatically recalculate whenever the dependent signal changes.**

---

This is your **fully restored Angular notes document** with additional sections for **Angular Components** and **Signals with Computed Properties**! 🚀 Let me know if you need any modifications or new topics. 😊

