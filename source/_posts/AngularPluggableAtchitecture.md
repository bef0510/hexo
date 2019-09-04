---
title: Angular Pluggable Architecture
date: 2019-09-04 14:59:50
tags:
 - Angular Advanced
categories: 
 - Angular
---

# Angular Pluggable Architecture
1. **Create a new project**
`ng new mainApp`

### Part plugin-a
1. **Add folder beside with mainApp**
`plugins → plugin-a`
![Architecture](1.png)

2. **Add package.json**
~~~ bash
{
    "name": "plugin-a",
    "version": "1.0.0",
    "license": "MIT",
    "scripts": {
        "build": "rollup -c"
    },
    "dependencies": {
        "@angular/common": "5.2.0",
        "@angular/core": "5.2.0",
        "rollup": "0.55.1",
        "rxjs": "5.5.6"
    },
    "devDependencies": {
        "rollup-plugin-angular": "^0.5.3",
        "rollup-plugin-commonjs": "8.3.0",
        "rollup-plugin-node-resolve": "3.0.2",
        "rollup-plugin-typescript": "0.8.1",
        "rollup-plugin-typescript2": "0.10.0",
        "typescript": "2.5.3"
    }
}
~~~

3. **Add tsconfig.json**
~~~ bash
{
    "compileOnSave": false,
    "compilerOptions": {
        "baseUrl": "./",
        "outDir": "./dist/out-tsc",
        "sourceMap": true,
        "declaration": false,
        "module": "es2015",
        "moduleResolution": "node",
        "emitDecoratorMetadata": true,
        "experimentalDecorators": true,
        "target": "es5",
        "typeRoots": [
            "node_modules/@types"
        ],
        "lib": [
            "es2017",
            "dom"
        ]
    }
}
~~~

4. **install**
`npm install`

5. **Add folder scr/app**
![Architecture](2.png)

6. **Add plugin-a.component.ts upder folder app**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'plugin-a-component',
  template: `<h3>Hi, I am the Plugin A component.</h3>
  <h4>Hi, I am the Plugin A component.</h4>`
})
export class PluginAComponent {
  constructor() { }
}
~~~

7. **Add plugin-a.module.ts upder folder app**
~~~ bash
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { PluginAComponent } from './plugin-a.component';

@NgModule({
  imports: [
    CommonModule
  ],
  declarations: [PluginAComponent],
  entryComponents: [PluginAComponent],
  providers: [{
     provide: 'plugins',
     useValue: [{
       name: 'plugin-a-component',
       component: PluginAComponent
     }],
     multi: true
   }]
})
export class PluginAModule { }
~~~

8. **Add main.ts upder folder src**
~~~ bash
export { PluginAModule } from './app/plugin-a.module';
~~~

9. **Add rollup.config.js under folder plugin-a**
~~~ bash
import angular from 'rollup-plugin-angular';
import resolve from 'rollup-plugin-node-resolve';
import typescript from 'rollup-plugin-typescript2';
import commonjs from 'rollup-plugin-commonjs';

export default [{
   input: 'src/main.ts',
   output: {
     file: '../../mainApp/src/assets/plugins/plugin-a.bundle.js',   
     format: 'umd',
     name: 'plugin-a',
},
   plugins: [
     angular(),
     resolve({
        jsnext: true,
        main: true,
        // pass custom options to the resolve plugin
        customResolveOptions: {
           moduleDirectory: 'node_modules'
        }
     }),
     typescript({
       typescript: require('typescript')
     }),
     commonjs()
   ],
   external: [
     '@angular/core',
     '@angular/common'
   ]
}]
~~~

10. **Download rollup**
`npm install rollup -g`

11. **Build plugin-a**
`npm run build`
![Architecture](3.png)

### Part mainApp
1. **Make sure bundle.js already exists under assets**
![Architecture](4.png)

2. **Download systemjs**
`npm install systemjs@0.21.3`

3. **angular.json**
~~~ bash
***
"scripts": [
    "./node_modules/systemjs/dist/system.js"
]
***
~~~

4. **main.ts**
~~~ bash
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

declare const SystemJS;
import * as angularCore from '@angular/core';
import * as angularCommon from '@angular/common';
SystemJS.set('@angular/core', SystemJS.newModule(angularCore));
SystemJS.set('@angular/common', SystemJS.newModule(angularCommon));

if (environment.production) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.error(err));
~~~

5. **app.module.ts**
~~~ bash
import { BrowserModule } from '@angular/platform-browser';
import { COMPILER_OPTIONS, CompilerFactory, Compiler, NgModule } from '@angular/core';
import { JitCompilerFactory } from '@angular/platform-browser-dynamic';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

export function createCompiler(fn: CompilerFactory): Compiler {     
  return fn.createCompiler();
}

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [{
    provide: COMPILER_OPTIONS,
    useValue: {},
    multi: true
  },
  {
    provide: CompilerFactory,
    useClass: JitCompilerFactory,
    deps: [COMPILER_OPTIONS]
  },
  {
    provide: Compiler,
    useFactory: createCompiler,
    deps: [CompilerFactory]
  }],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~

6. **app.component.html**
~~~ bash
<div #content></div>
~~~

7. **app.component.ts**
~~~ bash
import {
  AfterViewInit, Component, Compiler, Injector, ViewChild,
  ViewContainerRef
} from '@angular/core';

declare const SystemJS: any;

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements AfterViewInit {

  @ViewChild('content', { read: ViewContainerRef, static: false }) content: ViewContainerRef;

  constructor(private _compiler: Compiler, private _injector: Injector) { }

  ngAfterViewInit() {
    this.loadPlugins();
  }

  private async loadPlugins() {
    // import external module bundle
    const module = await SystemJS.import("assets/plugins/plugin-a.bundle.js");

    // compile module
    const moduleFactory = await this._compiler
      .compileModuleAsync<any>(module["PluginAModule"]);

    // resolve component factory
    const moduleRef = moduleFactory.create(this._injector);

    //get the custom made provider name 'plugins' 
    const componentProvider = moduleRef.injector.get('plugins');

    //from plugins array load the component on position 0 
    const componentFactory = moduleRef.componentFactoryResolver
      .resolveComponentFactory<any>(
        componentProvider[0][0].component
      );

    // compile component 
    var pluginComponent = this.content.createComponent(componentFactory);

    //sending @Input() values 
    //pluginComponent.instance.anyInput = "inputValue"; 

    //accessing the component template view
    //(pluginComponent.hostView as EmbeddedViewRef<any>).rootNodes[0] as HTMLElement;
  }
}
~~~

8. **Build mainApp**
`ng s`

## Change immediate
### Part plugin-a
1. **plugin-a.component.ts**
![Architecture](5.png)

2. **Build plugin-a**
`npm run build`

# Reference
[Pluggable Angular Apps - Paul Spears - Angular Lunch April 2019](https://www.youtube.com/watch?v=TEQdl3Pigiw)
[How to build a plugin/extensible application architecture in Angular 5+](https://itnext.io/how-to-build-a-plugin-extensible-application-architecture-in-angular5-736890278f3f)