---
title: Angular Binding
date: 2019-07-15 17:52:53
tags:
 - Angular Basic
categories: 
 - Angular
---

# Binding
1. **Interpolation**
2. **Property Binding**
3. **Event Binding**
4. **Two-way Binding**
5. **Class Binding**
6. **Style Binding**
7. **Event Binding**
![Architecture](1.png)
![Architecture](2.png)

## import FormsModule
![Architecture](3.png)

#### Interpolation { { } }
1. **app.component.ts**
~~~ bash
export class AppComponent {
  pageTitle = 'helloworld';
}
~~~

2. **app.component.html**
~~~ bash
<h1>{{ pageTitle }}</h1>
~~~

## Property Binding []
#### Property Binding 1
1. **app.component.ts**
~~~ bash
export class AppComponent {
  imgUrl = 'https://picsum.photos/200';
}
~~~

2. **app.component.html**
~~~ bash
<div>
  <img [src]="imgUrl" alt="Image" />
</div>
~~~

#### Property Binding 2
1. **test.component.ts**
~~~ bash
@Component({
  selector: 'app-test',
  template: `
    <h2>
      Welcome {{ name }}
    </h2>
    <input [id]="myId" type="text" value="NickName" />
    <input id="{{myId}}" type="text" value="NickName" />

    <input disabled="false" type="text" value="NickName" />
    <input disabled="{{false}}" type="text" value="NickName" />

    <input [disabled]="isDisabled" type="text" value="NickName" />
    <input bind-disabled="isDisabled" type="text" value="NickName" />

    <button (click)="onChangeDisabled()">Click</button>
  `,
  styleUrls: []
})
export class TestComponent implements OnInit {

  public name = "Hello World";
  public myId = "testId";
  public isDisabled = true;

  constructor() { }

  ngOnInit() {
  }

  onChangeDisabled() {
    this.isDisabled = !this.isDisabled;
  }
}
~~~
2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### Event Binding ()
1. **app.component.ts**
~~~ bash
export class AppComponent {
  count = 0;

  incrementCount() {
    this.count += 1;
  }
}
~~~

2. **app.component.html**
~~~ bash
<div>
  <button (click)="incrementCount()">Click</button>
  <p>Clicked {{ count }} times</p>
</div>
~~~

## Two-way Binding [()]
#### Two-way Binding 1
1. **app.component.ts**
~~~ bash
export class AppComponent {
  userName: string;
}
~~~

2. **app.component.html**
~~~ bash
<div>
  <input type="text" [ngModel]="userName" (ngModelChange)="userName=$event">
  <p>Welcome {{ userName }}</p>
</div>
~~~

#### Two-way Binding 2
1. **app.component.ts**
~~~ bash
export class AppComponent {
  nickName: string;

  greetNickName(updatedValue) {
    this.nickName = updatedValue;

    if(updatedValue == 'Nickname'){
      alert('Welcome back Nickname');
    }
  }
}
~~~

2. **app.component.html**
~~~ bash
<div>
  <input type="text" [ngModel]="nickName" (ngModelChange)="greetNickName($event)">
  <p>Welcome {{ nickName }}</p>
</div>
~~~

#### Two-way Binding 3
1. **app.component.ts**
~~~ bash
export class AppComponent {
  name: string;
}
~~~

2. **app.component.html**
~~~ bash
<div>
  <input type="text" [(ngModel)]="name">
  <p>Welcome {{ name }}</p>
</div>
~~~

#### Class Binding
1. **test.component.ts**
~~~ bash
@Component({
  selector: 'app-test',
  template: `
    <h2>
      Welcome {{ name }}
    </h2>
    <h2 class="text-success">Hello</h2>
    <h2 [class]="successClass">Hello</h2>

    <h2 class="text-special" [class]="successClass">Hello</h2> <!--text-special not work-->

    <h2 [class.text-danger]="hasError">Hello</h2> <!--single class-->

    <h2 [ngClass]="messageClasses">Hello</h2> <!--multiple classes-->

    <button (click)="onChangeClass()">Click</button>
  `,
  styles: [`
    .text-success {
      color: green;
    }
    .text-danger {
      color: red;
    }
    .text-special {
      font-style: italic;
    }
  `]
})
export class TestComponent implements OnInit {

  public name = "Hello World";
  public successClass = "text-success"
  public hasError = false;
  public isSpecial = true;
  public messageClasses = {
    "text-success": !this.hasError,
    "text-danger": this.hasError,
    "text-special": this.isSpecial
  }

  constructor() { }

  ngOnInit() {
  }

  onChangeClass() {
    this.hasError = !this.hasError;
    this.messageClasses["text-danger"] = this.hasError;
  }
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### Style Binding
1. **test.component.ts**
~~~ bash
@Component({
  selector: 'app-test',
  template: `
    <h2>
      Welcome {{ name }}
    </h2>

    <h2 [style.color]="'orange'">Style Binding</h2>
    <h2 [style.color]="hasError ? 'red' : 'green'">Style Binding</h2>
    <h2 [style.color]="highlightColor">Style Binding 2</h2>
    
    <h2 [ngStyle]="titleStyles">Style Binding 3</h2>

    <button (click)="onChangeClass()">Click</button>
  `,
  styles: []
})
export class TestComponent implements OnInit {

  public name = "Hello World";
  public hasError = false;
  public highlightColor = "orange";
  public titleStyles = {
    color: "blue",
    fontStyle: "italic"
  }

  constructor() { }

  ngOnInit() {
  }

  onChangeClass() {
    this.hasError = !this.hasError;
  }
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### Event Binding
1. **test.component.ts**
~~~ bash
@Component({
  selector: 'app-test',
  template: `
    <h2>
      Welcome {{ name }}
    </h2>

    <button (click)="onClick()">Click Function</button>
    {{ greeting }}

    <button (click)="onClickEvent($event)">Click Event</button>
    {{ greetingEvent }}

    <button (click)="greeting = 'Hello'">Click</button>
  `,
  styles: []
})
export class TestComponent implements OnInit {

  public name = "Hello World";
  public greeting = "";
  public greetingEvent = "";

  constructor() { }

  ngOnInit() {
  }

  onClick() {
    console.log(this.name);
    this.greeting = this.name;
  }

  onClickEvent(event) {
    console.log(event);
    this.greetingEvent = event.type;
  }
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

# Reference
[Angular Component Interaction - 3 - Binding](https://www.youtube.com/watch?v=rH-xfNvpm_o&list=PLC3y8-rFHvwgKhaLU8GTyF-5Bb8qT-wzV&index=3)
[Angular 8 Tutorial - 6 - Property Binding](https://www.youtube.com/watch?v=N8FBmB2jme8&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=6)
[Angular 8 Tutorial - 7 - Class Binding](https://www.youtube.com/watch?v=Y6OP-lPJxgs&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=7)
[Angular 8 Tutorial - 8 - Style Binding](https://www.youtube.com/watch?v=q256X6-u9Q8&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=8)
[Angular 8 Tutorial - 9 - Event Binding](https://www.youtube.com/watch?v=ZfIc1_oj7uM&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=9)