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

# 📌 **Content Projection in Angular**

## 🔹 **What is Content Projection?**
Content projection in Angular allows you to **pass custom content (HTML) from a parent component into a child component** using `<ng-content>`. This is useful when creating **reusable UI components** like modals, cards, or buttons where the structure is predefined, but the content is dynamic.

---

## 🔹 **Basic Example of Content Projection**
Imagine you have a **Card Component** where the **title and content** can be dynamically provided by the parent.

### ✅ **Step 1: Define `ng-content` in the Child Component (`card.component.html`)**
```html
<div class="card">
  <h3>Card Title</h3>
  <ng-content></ng-content>  <!-- Content from parent will be placed here -->
</div>
```

### ✅ **Step 2: Use `app-card` in Parent Component**
```html
<app-card>
  <p>This is dynamic content inside the card.</p>
</app-card>
```
**🔹 Output:**  
```
[ Card Title ]
-------------------
| This is dynamic content inside the card. |
```
✅ **The paragraph `<p>` inside `<app-card>` is projected into `<ng-content>` in `card.component.html`.**  

---

## 🔹 **Multi-Slot Content Projection**
You can have **multiple projection slots** using the `select` attribute.

### ✅ **Step 1: Modify `card.component.html` to Support Multiple Slots**
```html
<div class="card">
  <h3><ng-content select="[card-title]"></ng-content></h3>
  <p><ng-content select="[card-body]"></ng-content></p>
</div>
```
- **`select="[card-title]"`** → Projects only elements with `card-title` attribute.
- **`select="[card-body]"`** → Projects only elements with `card-body` attribute.

### ✅ **Step 2: Use Slots in the Parent Component**
```html
<app-card>
  <span card-title>Custom Card Title</span>
  <p card-body>This is the card content.</p>
</app-card>
```
**🔹 Output:**
```
[ Custom Card Title ]
-------------------
| This is the card content. |
```
✅ **Only elements with matching attributes are projected into specific slots.**

---

## 🔹 **Conditional Projection (`ng-content` with `ngIf`)**
You can also **conditionally project content** using `*ngIf`.

### ✅ **Example**
```html
<div class="card">
  <h3><ng-content select="[card-title]"></ng-content></h3>
  <p *ngIf="showBody"><ng-content select="[card-body]"></ng-content></p>
</div>
```
- If `showBody = true`, it **shows the projected content** inside `[card-body]`.
- If `showBody = false`, it **hides the content**.

---

## 🔹 **When to Use Content Projection?**
✅ When creating **reusable UI components** (e.g., cards, modals, alerts).  
✅ When you **don't know the exact content** beforehand.  
✅ When you need **multiple projection slots** for flexibility.

---

### 🔥 **Final Summary**
| Feature | Syntax | Purpose |
|---------|--------|---------|
| **Single Slot** | `<ng-content></ng-content>` | Pass any content inside a component |
| **Multi-Slot** | `<ng-content select="[slot-name]"></ng-content>` | Pass different sections using attributes |
| **Conditional Slot** | `<ng-content *ngIf="condition"></ng-content>` | Show or hide projected content dynamically |

# 📌 **Angular Pipes**
### 🔹 **What are Pipes?**
Pipes in Angular are **used to transform** displayed data in templates without modifying the original data.  
They help format **text, numbers, dates, and more**.

---

## 🔹 **1. Built-in Pipes in Angular**
Angular provides several **built-in pipes**:

| Pipe | Purpose | Example | Output |
|------|---------|---------|--------|
| **`uppercase`** | Converts text to uppercase | `{{ 'hello' | uppercase }}` | `HELLO` |
| **`lowercase`** | Converts text to lowercase | `{{ 'HELLO' | lowercase }}` | `hello` |
| **`titlecase`** | Capitalizes the first letter of each word | `{{ 'angular pipes' | titlecase }}` | `Angular Pipes` |
| **`date`** | Formats dates | `{{ today | date:'short' }}` | `2/27/25, 10:00 AM` |
| **`currency`** | Formats numbers as currency | `{{ 1000 | currency:'USD' }}` | `$1,000.00` |
| **`percent`** | Converts numbers to percentage | `{{ 0.25 | percent }}` | `25%` |
| **`json`** | Displays JSON objects as a string | `{{ user | json }}` | `{"name": "John", "age": 25}` |
| **`slice`** | Extracts a portion of an array or string | `{{ 'Angular' | slice:0:3 }}` | `Ang` |

---

## 🔹 **2. Using Pipes in Angular**
Pipes are **used inside templates** with the `|` (pipe) symbol.

### ✅ **Example: Using Built-in Pipes**
```html
<p>Uppercase: {{ 'hello' | uppercase }}</p>
<p>Lowercase: {{ 'HELLO' | lowercase }}</p>
<p>Title Case: {{ 'angular pipes' | titlecase }}</p>
<p>Formatted Date: {{ today | date:'fullDate' }}</p>
<p>Currency: {{ 1000 | currency:'USD' }}</p>
```

#### **In Component (`.ts` file):**
```typescript
export class ExampleComponent {
  today = new Date();
}
```

**🔹 Output:**
```
Uppercase: HELLO
Lowercase: hello
Title Case: Angular Pipes
Formatted Date: Thursday, February 27, 2025
Currency: $1,000.00
```

---

## 🔹 **3. Chaining Multiple Pipes**
You can apply **multiple pipes** in a single expression:

### ✅ **Example**
```html
<p>Formatted Price: {{ 2500 | currency:'USD':'symbol' | uppercase }}</p>
```
**Output:** `$2,500.00`

---

## 🔹 **4. Parameterized Pipes**
Some pipes **accept parameters** to customize the output.

### ✅ **Example: `date` Pipe with Parameters**
```html
<p>Short Date: {{ today | date:'shortDate' }}</p>
<p>Full Date: {{ today | date:'fullDate' }}</p>
<p>Custom Format: {{ today | date:'dd/MM/yyyy' }}</p>
```
**🔹 Output:**
```
Short Date: 2/27/25
Full Date: Thursday, February 27, 2025
Custom Format: 27/02/2025
```

---

## 🔹 **5. Creating a Custom Pipe**
You can create **your own pipe** using the `@Pipe` decorator.

### ✅ **Example: Creating a Pipe to Reverse Strings**
#### **Step 1: Generate a Pipe**
```sh
ng generate pipe reverse
# or shorthand
ng g p reverse
```
#### **Step 2: Implement the Pipe (`reverse.pipe.ts`)**
```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'reverse'
})
export class ReversePipe implements PipeTransform {
  transform(value: string): string {
    return value.split('').reverse().join('');
  }
}
```
#### **Step 3: Use the Pipe in a Template**
```html
<p>Reversed: {{ 'Angular' | reverse }}</p>
```
**Output:**  
```
Reversed: ralugnA
```

---

## 🔹 **6. Pure vs Impure Pipes**
### ✅ **Pure Pipe (Default)**
- Only re-executes if **input value changes**.
- **Faster performance**.
- Used for **static transformations** (e.g., uppercase, date).

✅ **Example: Default Pure Pipe**
```typescript
@Pipe({ name: 'reverse' })
export class ReversePipe implements PipeTransform {
  transform(value: string): string {
    return value.split('').reverse().join('');
  }
}
```

### ❌ **Impure Pipe**
- Runs **on every change detection cycle**.
- Used for **dynamic transformations** (e.g., filtering lists).
- **Can slow down performance**.

✅ **Example: Marking a Pipe as Impure**
```typescript
@Pipe({
  name: 'filter',
  pure: false // ❗ Runs on every update
})
export class FilterPipe implements PipeTransform {
  transform(tasks: any[], searchTerm: string): any[] {
    return tasks.filter(task => task.title.includes(searchTerm));
  }
}
```
---

## 🔥 **Final Summary**
| Feature | Example | Purpose |
|---------|---------|---------|
| **Built-in Pipes** | `uppercase`, `date`, `currency`, `json` | Predefined transformations |
| **Chaining Pipes** | `{{ price | currency | uppercase }}` | Apply multiple pipes |
| **Parameterized Pipes** | `{{ today | date:'short' }}` | Customize format |
| **Custom Pipes** | `@Pipe({ name: 'reverse' })` | Define custom behavior |
| **Pure vs Impure Pipes** | `pure: false` in `@Pipe` | Performance optimization |


### Summary of Angular Lifecycle Hooks:

| Lifecycle Hook             | When It's Called                                | Purpose |
|----------------------------|------------------------------------------------|---------|
| **`ngOnChanges`**           | Before `ngOnInit` and whenever input properties change. | Respond to changes in `@Input` properties. |
| **`ngOnInit`**              | Once after the first `ngOnChanges`.            | Perform initialization tasks (e.g., API calls). |
| **`ngDoCheck`**             | During every change detection cycle.           | Custom change detection logic. |
| **`ngAfterContentInit`**    | After content is projected into the component. | Initialize content passed via `ng-content`. |
| **`ngAfterContentChecked`** | After every check of projected content.        | Post-processing after content checking. |
| **`ngAfterViewInit`**       | After the component’s view and its children are initialized. | Perform tasks related to the view and child components. |
| **`ngAfterViewChecked`**    | After every check of the component’s view and child views. | Post-processing after view checks. |
| **`ngOnDestroy`**           | Just before the component is destroyed.       | Clean up resources and unsubscribe from observables. |

Each lifecycle hook allows you to hook into a specific point during the component’s lifecycle, giving you flexibility and control over your component’s behavior.
