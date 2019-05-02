---
title: Router
date: 2019-04-30 17:22:21
tags:
 - Angular
categories: 
 - Angular
---

# 使用路由
`app.component.html` 修改成下列程式

    <div class="container">
        <nav class="navbar navbar-default">
            <ul class="nav navbar-nav">
                <li>
                    <a routerLinkActive="active" routerLink="list">List</a>
                </li>
                <li>
                    <a routerLinkActive="active" routerLink="create">Create</a>
                </li>
            </ul>
        </nav>
        <router-outlet></router-outlet>
    </div>

加入三個 **Component**
~~~ bash
$ ng g c list --spec false
$ ng g c create --spec false
$ ng g c not-fond --spec false
~~~

加入路由
~~~ bash
$ ng g m app-routing --flat --module=app
~~~
`app-routing.module.ts` 修改成下列程式

    import { NgModule } from '@angular/core';
    import { Routes, RouterModule } from '@angular/router';
    import { ListStudentsComponent } from './student/list-students/list-students.component';
    import { CreateStudentComponent } from './student/create-student/create-student.component';
    import { ErrorPageComponent } from './error-page/error-page.component';

    const routes: Routes = [
        { path: 'list', component: ListStudentsComponent },
        { path: 'create', component: CreateStudentComponent },
        { path: '', redirectTo: '/list', pathMatch: 'full' },
        { path: 'not-found', component: ErrorPageComponent },
        { path: '**', redirectTo: '/not-found' }
    ];

    @NgModule({
        declarations: [],
        imports: [ RouterModule.forRoot(routes) ],
        exports: [RouterModule]
    })
    export class AppRoutingModule { }

啟動 **Server** 確認
~~~ bash
$ ng s
~~~
![Router](1.gif)

# Reference
[Angular-generate](https://angular.io/cli/generate)
[Angular-Routing](https://angular.io/tutorial/toh-pt5)