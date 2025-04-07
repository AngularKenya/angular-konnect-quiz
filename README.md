# Angular Konnekt Quiz

## Quiz Structure
- **Format**: Mix of one-answer questions and bug-fixing challenges  
- **Team Setup**: 3–5 members per team recommended (Depends on Attendees)  
---

## One-Answer Questions

1. **Which scenario best suits the use of `ng-content` with `@ContentChild()`?**  
   - A. Projecting child component into the parent's template  
   - B. Styling parent from the child component  
   - C. Interacting with projected DOM elements or templates from the parent  
   - D. Passing data between unrelated components  
   - **Answer**: C  

2. **When using a shared service with a `BehaviorSubject`, what's a key feature of `BehaviorSubject`?**  
   - A. It only emits values to subscribers after a delay  
   - B. It stores the last emitted value and sends it immediately to new subscribers  
   - C. It can emit values only once  
   - D. It works only with `@Output()`  
   - **Answer**: B  

3. **What is the main difference between `ViewChild` and `ContentChild` in Angular?**  
   - A. `ViewChild` is for projected content; `ContentChild` is for DOM children  
   - B. `ViewChild` is for component elements; `ContentChild` is for projections  
   - C. `ViewChild` works only with services; `ContentChild` works with DOM  
   - D. There is no difference  
   - **Answer**: B  

4. **What is the best way to communicate between sibling components?**  
   - A. Using `@Input()` and `@Output()`  
   - B. Using shared service with `Subject`/`BehaviorSubject`  
   - C. Using `ViewChild`  
   - D. Using Router parameters  
   - **Answer**: B  

5. **In which lifecycle hook can you best detect changes in `@Input()` properties?**  
   - A. `ngOnInit()`  
   - B. `ngAfterViewInit()`  
   - C. `ngOnChanges()`  
   - D. `ngDoCheck()`  
   - **Answer**: C  

6. **How does TypeScript infer types, and how can you override the inferred types when needed?**  
   - A. TypeScript infers types automatically based on assigned values, and you can override this inference with type annotations or type assertions  
   - B. TypeScript infers types based only on variable names, and you cannot override the inferred types  
   - C. TypeScript infers types based on function parameters, and you can override this inference by using any type  
   - D. TypeScript never infers types, and you must always manually declare types  
   - **Answer**: A  

### TypeScript Type Inference Examples
```typescript
let name = "Alice"; // inferred as string
let count = 42;     // inferred as number
let active = true;  // inferred as boolean

function greet() {
  return "Hello"; // inferred as string
}
```
Override Methods

- Explicitly specify the type
- Type Assertions (Casting) - Use "as" to tell TypeScript the exact type
- Generics - When inference isn't enough in functions or components

### Mapped Types in TypeScript



7. ### Which of the following is an example of a Mapped Type


```typescript  
A) type MyType = string | number 
```

```typescript  
B) type ReadOnlyPerson = { readonly [K in keyof Person]: Person[K] };
```

```typescript  
C) function myFunction() { return true; }
```

```typescript 
D) interface Car { make: string; model: string; } 
```

 - **Answer**: B  
Mapped types allow you to create new types by transforming existing ones — often based on the properties of another type.

```typescript
type Person = {
  name: string;
  age: number;
};

// Make all properties optional
type PartialPerson = {
  [K in keyof Person]?: Person[K];
};

// Equivalent to
type PartialPerson = {
  name?: string;
  age?: number;
};

```

8. ### What is a Subject in RxJS, and how does it differ from an Observable?
- A. A Subject is an Observable that emits values to multiple subscribers simultaneously, whereas an Observable emits values to each subscriber individually
- B. A Subject and an Observable are exactly the same and can be used interchangeably
- C. A Subject only emits values to a single subscriber, while an Observable can emit values to multiple subscribers
- D. A Subject is a special type of Observable that only supports synchronous value emission, whereas an Observable can emit asynchronous values
- **Answer**: A

9. ### What is the purpose of using resolve in Angular routing?
- A. To control navigation flow based on user authentication
- B. To pre-fetch and load data before a route is activated
- C. To redirect the user to another route if certain conditions are met
- D. To protect routes from unauthorized access
- **Answer**: B


10. ### Which is false regarding Angular Signals and RxJS Observables?


- A. Angular Signals are simpler and automatically update reactive values, while RxJS Observables are more flexible and support complex operations
- B. Angular Signals require complex operators to manage streams, while RxJS Observables are simple to use
- C. Angular Signals can represent multiple asynchronous data streams, while RxJS Observables can only handle simple state management
- D. Angular Signals are used only for HTTP responses, while RxJS 
Observables are used for component state management

- **Answer**: B


11. ### What is the difference between an Injector and a Provider in Angular?

- A. A provider configures how a dependency should be created, while an injector resolves and injects it at runtime
- B. A provider is responsible for injecting dependencies, while an injector configures how they should be created
- C. A provider is only used in services, while an injector is used in components
- D. An injector creates dependencies, but a provider only provides static values
- **Answer**: A

## Bug Snippet Challenges

### Bug Snippet 1: Component Communication
```typescript 
@Component({
  selector: 'app-parent',
  template: `
    <h2>Parent Component</h2>
    <app-child [data]="parentData" (dataChange)="handleDataChange()"></app-child>
  `
})
export class ParentComponent {
  parentData = 'Initial data';
 
  handleDataChange(newValue) {
    this.parentData = newValue;
  }
}

@Component({
  selector: 'app-child',
  template: `
    <h3>Child Component</h3>
    <p>Data from parent: {{ data }}</p>
    <button (click)="updateData()">Update Data</button>
  `
})
export class ChildComponent {
  @Input() data: string;
  @Output() dataChange = new EventEmitter<string>();
 
  updateData() {
    dataChange.emit('Updated data');
  }
}
```
**Bug**: The event emitter isn't working correctly. What's wrong?

**Answer**: Missing this keyword in `updateData()` method. Should be `this.dataChange.emit('Updated data')`;


## Bug Snippet 2: Dependency Injection
```typescript
@Injectable()
export class DataService {
  getData() {
    return ['Item 1', 'Item 2', 'Item 3'];
  }
}

@Component({
  selector: 'app-data-display',
  template: `
    <ul>
      <li *ngFor="let item of items">{{ item }}</li>
    </ul>
  `,
  providers: [DataService]
})
export class DataDisplayComponent implements OnInit {
  items: string[];
 
  constructor(private dataService: DataService) { }
 
  ngOnInit() {
    this.items = this.dataService.getData;
  }
} 
```
**Bug**: The data isn't displaying. What's wrong?

**Answer**: Missing function call parentheses in `ngOnInit()`. Should be `this.items = this.dataService.getData()`;

### Bug Snippet 3: Form Validation
``` typescript
@Component({
  selector: 'app-registration',
  template: `
    <form [formGroup]="registerForm" (ngSubmit)="onSubmit()">
      <div>
        <label>Email</label>
        <input type="email" formControlName="email">
        <div *ngIf="registerForm.get('email').invalid && registerForm.get('email').touched">
          Email is required and must be valid
        </div>
      </div>
      <button type="submit" [disabled]="registerForm.invalid">Register</button>
    </form>
  `
})
export class RegistrationComponent implements OnInit {
  registerForm: FormGroup;
 
  constructor(private fb: FormBuilder) { }
 
  ngOnInit() {
    this.registerForm = this.fb.group({
      email: ['', [Validators.required, Validators.email]]
    });
  }
 
  onSubmit() {
    console.log(this.registerForm.value);
  }
}
```

**Bug**: The application crashes when the form loads. What's wrong?

**Answer**: Missing safe navigation operator in the template. Should be `*ngIf="registerForm.get('email')?.invalid && registerForm.get('email')?.touched"`


### Bug Snippet 4: Observable Subscription
```typescript
@Component({
  selector: 'app-user-list',
  template: `
    <h2>User List</h2>
    <ul>
      <li *ngFor="let user of users">{{ user.name }}</li>
    </ul>
  `
})
export class UserListComponent implements OnInit {
  users: User[] = [];
 
  constructor(private userService: UserService) { }
 
  ngOnInit() {
    this.userService.getUsers().subscribe(users => {
      this.users = users;
    });
  }
}
```
**Bug**: This component has a potential memory leak. What's missing?

**Answer**: Missing unsubscribe. Should implement OnDestroy and unsubscribe from the subscription.

## Bug Snippet 5: Change Detection
```typescript
@Component({
  selector: 'app-counter',
  template: `
    <h2>Counter: {{ count }}</h2>
    <button (click)="increment()">Increment</button>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class CounterComponent {
  count = 0;
 
  increment() {
    this.count++;
  }
} 
```
**Bug**: The counter doesn't update when clicking the button. What's wrong?

**Answer**: Using OnPush change detection but not triggering change detection when count updates. Either switch to Default change detection or inject ChangeDetectorRef and call detectChanges() after updating count.
**Answer 2**: Since ChangeDetectionStrategy.OnPush is used, Angular only detects changes when a new object reference is assigned. The solution is to use this.count = this.count + 1; instead of this.count++.


