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



## 📌 Angular Data Binding

### 🔹 What is Data Binding?

Data binding is a mechanism that connects the **UI (HTML)** with the **TypeScript class (Component)**. It ensures data synchronization between them.

### 🔹 Types of Data Binding

| Type | Syntax | Purpose |
|------|--------|---------|
| **Interpolation** | `{{ value }}` | Display dynamic data in the template |
| **Property Binding** | `[property]="value"` | Bind element properties dynamically |
| **Event Binding** | `(event)="function()"` | Handle user actions (click, input, etc.) |
| **Two-Way Binding** | `[(ngModel)]="value"` | Sync input field with a component variable |

#### 1️⃣ **Interpolation (`{{}}`)**
```html
<h1>{{ title }}</h1>
```

#### 2️⃣ **Property Binding (`[]`)**
```html
<img [src]="imageUrl" alt="Logo">
```

#### 3️⃣ **Event Binding (`()`)**
```html
<button (click)="increment()">Increase</button>
```

#### 4️⃣ **Two-Way Binding (`[()]`)**
```html
<input [(ngModel)]="name">
```

---

## 📌 `@Input()` and `@Output()` in Angular

### 🔹 `@Input()` - Passing Data from Parent to Child

- `@Input()` is a decorator in Angular used to pass **data from a parent component to a child component**.

#### **Example: Using `@Input()`**
```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent {
  @Input() name!: string;  
  @Input() avatar!: string;
}
```

#### **Usage in Parent Component**
```html
<app-child [name]="user.name" [avatar]="user.avatar"></app-child>
```

### 🔹 `@Output()` - Emitting Events from Child to Parent

- `@Output()` allows a **child component** to send an event to its **parent component**.

#### **Example: Using `@Output()`**
```typescript
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent {
  @Output() notifyParent = new EventEmitter<string>();  

  sendMessage() {
    this.notifyParent.emit('Hello Parent!');  
  }
}
```

#### **Usage in Parent Component**
```html
<app-child (notifyParent)="receiveMessage($event)"></app-child>
<p>Message: {{ messageFromChild }}</p>
```

---

## 📌 `@for`, `@if-else`, `*ngFor`, and `*ngIf-else` Directives in Angular

### 🔹 `@for` Directive (Looping in Templates)
```html
<ul>
  @for (user of users; track user.id) {
    <li>{{ user.name }}</li>
  }
</ul>
```

### 🔹 `@if-else` Directive (Conditional Rendering)
```html
@if (isLoggedIn) {
  <p>Welcome back!</p>
} @else {
  <p>Please log in.</p>
}
```

### 🔹 `*ngFor` Directive
```html
<ul>
  <li *ngFor="let user of users; trackBy: trackById">{{ user.name }}</li>
</ul>
```

### 🔹 `*ngIf-else` Directive
```html
<div *ngIf="isLoggedIn; else loggedOut">
  <p>Welcome back!</p>
</div>
<ng-template #loggedOut>
  <p>Please log in.</p>
</ng-template>
```

---
### 📌 **All Angular Directives (Structural & Attribute)**

In Angular, **directives** are special markers on elements that tell Angular to **modify the structure or behavior of the DOM**. There are two main types:

1. **Structural Directives** → Modify the DOM layout (e.g., adding/removing elements).  
2. **Attribute Directives** → Modify the appearance or behavior of elements.

---

## 🔹 **1. Structural Directives**
**Structural directives** are **prefixed with `*`** and change the **DOM layout** by adding, removing, or manipulating elements.

| Directive  | Purpose  | Example  |
|------------|----------|----------|
| `*ngIf` | Conditionally renders elements | `<p *ngIf="isLoggedIn">Welcome</p>` |
| `*ngIf else` | Renders alternative content | `<p *ngIf="isLoggedIn; else loggedOut">Welcome</p>` |
| `*ngFor` | Loops through an array to render elements | `<li *ngFor="let item of items">{{ item }}</li>` |
| `*ngSwitch` | Conditionally displays elements based on a value | `<div [ngSwitch]="role"><p *ngSwitchCase="'admin'">Admin</p></div>` |
| `@if` (New) | Alternative for `*ngIf` in newer Angular versions | `@if (isLoggedIn) { <p>Welcome</p> } @else { <p>Login</p> }` |
| `@for` (New) | Alternative for `*ngFor` | `@for (user of users) { <p>{{ user.name }}</p> }` |

### ✅ **Example of Structural Directives**
```html
<ul>
  <li *ngFor="let task of tasks; trackBy: trackTaskId">
    {{ task.title }}
  </li>
</ul>

<p *ngIf="isLoggedIn; else guestView">Welcome, User!</p>
<ng-template #guestView><p>Please log in.</p></ng-template>

<div [ngSwitch]="userRole">
  <p *ngSwitchCase="'admin'">Admin Panel</p>
  <p *ngSwitchCase="'user'">User Dashboard</p>
  <p *ngSwitchDefault>Guest View</p>
</div>
```

---

## 🔹 **2. Attribute Directives**
**Attribute directives** **modify the behavior or appearance** of an element.

| Directive | Purpose | Example |
|------------|----------|----------|
| `[ngClass]` | Dynamically adds/removes CSS classes | `<p [ngClass]="{ active: isActive }">Hello</p>` |
| `[ngStyle]` | Dynamically applies inline styles | `<p [ngStyle]="{ color: 'blue', fontSize: '20px' }">Styled Text</p>` |
| `[disabled]` | Dynamically enables/disables an element | `<button [disabled]="isDisabled">Click</button>` |
| `[(ngModel)]` | Two-way data binding in forms | `<input [(ngModel)]="name">` |

### ✅ **Example of Attribute Directives**
```html
<input [(ngModel)]="username" placeholder="Enter Name">

<p [ngClass]="{ 'highlighted': isHighlighted }">Dynamic Class</p>

<p [ngStyle]="{ color: isError ? 'red' : 'green' }">Conditional Styling</p>

<button [disabled]="!isFormValid">Submit</button>
```

---

## 🔹 **3. Custom Directives**
You can also create your **own directives**.

### ✅ **Example: Custom Directive to Change Background Color**
#### **Step 1: Create the Directive**
```typescript
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.el.nativeElement.style.backgroundColor = 'yellow';
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.el.nativeElement.style.backgroundColor = 'transparent';
  }
}
```
#### **Step 2: Use the Directive in a Template**
```html
<p appHighlight>Hover over me to change background color.</p>
```
✅ **Now, this directive will apply a background color change when hovered!**  

---

### 🔥 **Summary**
| Directive Type | Examples | Purpose |
|--------------|------------|----------|
| **Structural Directives** | `*ngIf`, `*ngFor`, `*ngSwitch`, `@if`, `@for` | Modify the structure of the DOM (add/remove elements) |
| **Attribute Directives** | `[ngClass]`, `[ngStyle]`, `[disabled]`, `[(ngModel)]` | Modify the behavior/appearance of an element |
| **Custom Directives** | `appHighlight` (Custom) | Create custom behaviors |



