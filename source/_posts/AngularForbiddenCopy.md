---
title: AngularForbiddenCopy
date: 2019-11-07 10:06:01
tags:
 - Angular Advanced
categories: 
 - Angular
---

# Angular Forbidden Copy (禁止複製)

1. **Add File block-copy-paste.directive.ts**
~~~ bash
import { Directive, HostListener } from '@angular/core';

@Directive({
  selector: '[appBlockCopyPaste]'
})
export class BlockCopyPasteDirective {
  constructor() { }

  @HostListener('paste', ['$event']) blockPaste(e: KeyboardEvent) {
    e.preventDefault();
  }

  @HostListener('copy', ['$event']) blockCopy(e: KeyboardEvent) {
    e.preventDefault();
  }

  @HostListener('cut', ['$event']) blockCut(e: KeyboardEvent) {
    e.preventDefault();
  }
}
~~~

2. **Using in .html**
~~~ bash
<div appBlockCopyPaste>
    test
</div>
~~~

3. **app.module.ts**
~~~ bash
...
import { BlockCopyPasteDirective } from './services/block-copy-paste.directive';
...

@NgModule({
  declarations: [
    ...
    BlockCopyPasteDirective,
    ...
  ],
  imports: [
    ...
  ],
  entryComponents: [
    ...
  ],
  providers: [
    ...
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~

# Reference
[使用Angular2禁用文本框的剪切,复制和粘贴功能的指令](http://www.voidcn.com/article/p-uvgccuek-bys.html)