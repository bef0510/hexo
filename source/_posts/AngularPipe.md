---
title: Angular Pipe
date: 2019-07-17 11:35:02
tags:
 - Angular Basic
categories: 
 - Angular
---

# Pipe
1. **UpperCasePipe**
2. **LowerCasePipe**
3. **TitleCasePipe**
4. **SlicePipe**
5. **JsonPipe**
6. **DecimalPipe**
7. **PercentPipe**
8. **CurrencyPipe**
9. **DatePipe**
10. **AsyncPipe**
11. **Customer**

#### UpperCasePipe
1. **test.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <div>
      {{ name | uppercase }}
    </div>
  `,
  styleUrls: []
})
export class TestComponent {
  public name = "Angular";
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### LowerCasePipe
1. **test.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <div>
      {{ name | lowercase }}
    </div>
  `,
  styleUrls: []
})
export class TestComponent {
  public name = "Angular";
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### TitleCasePipe
1. **test.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <div>
      {{ message | titlecase }}
    </div>
  `,
  styleUrls: []
})
export class TestComponent {
  public name = "Angular";
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### SlicePipe
1. **test.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <div>
      {{ name | slice:4 }}<br />
      {{ name | slice:4:6 }}
    </div>
  `,
  styleUrls: []
})
export class TestComponent {
  public name = "Angular";
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### JsonPipe
1. **test.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <div>
      {{ person | json }}
    </div>
  `,
  styleUrls: []
})
export class TestComponent {
  public person = {
    "firstName": "Joey",
    "lastName": "Tribbiani",
  }
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### DecimalPipe
1. **test.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <div>
      {{ math | number: "1.2-3" }}<br />
      {{ math | number: "3.4-5" }}<br />
      {{ math | number: "3.1-2" }}
    </div>
  `,
  styleUrls: []
})
export class TestComponent {
  public math = 5.678;
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### PercentPipe
1. **test.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <div>
      {{ math | percent }}
    </div>
  `,
  styleUrls: []
})
export class TestComponent {
  public math = 0.25;
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### CurrencyPipe
1. **test.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <div>
      {{ math | currency }}<br />
      {{ math | currency: "GBP"  }}<br />
      {{ math | currency: "GBP": "code"  }}<br />
      {{ math | currency: "TWD"  }}<br />
      {{ math | currency: "TWD": "code"  }}<br />
    </div>
  `,
  styleUrls: []
})
export class TestComponent {
  public math = 0.25;
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### DatePipe
1. **test.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <div>
      {{ date }}<br />
      {{ date | date:'short' }}<br />
      {{ date | date:'shortDate' }}<br />
      {{ date | date:'shortTime' }}<br />
      {{ date | date:'medium' }}<br />
      {{ date | date:'mmss' }}<br />
      {{ date | date:'yyyy-MM-dd hh:mm:ss' }}
    </div>
  `,
  styleUrls: []
})
export class TestComponent {
  public date = new Date();
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### AsyncPipe
1. **test.component.ts**
~~~ bash
import { Component } from '@angular/core';
import { Observable, Subscriber } from 'rxjs';

@Component({
  selector: 'app-test',
  template: `
    <div>
      Time: {{ time | async }}
    </div>
  `,
  styleUrls: []
})
export class TestComponent {
  public time = new Observable<string>((observer: Subscriber<string>) => {
    setInterval(() => observer.next(new Date().toString()), 1000);
  });
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### Customer

# Reference
[Angular 8 Tutorial - 16 - Pipes](https://www.youtube.com/watch?v=y8lwG8IM82k&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=16)