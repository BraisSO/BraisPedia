# TypeScript 
- `tsc -init` se utiliza para inicializar un proyecto TypeScript
- `tsc -w` se utiliza para iniciar el "watch mode" y compilar automáticamente los cambios en los archivos TypeScript
- `tsc (nombreArchivo)` se utiliza para compilar un archivo TypeScript específico de forma manual
- `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser` se utiliza para establecer los privilegios necesarios en PowerShell para ejecutar scripts TypeScript

# Angular 
- `ng new /nombre` se utiliza para iniciar un nuevo proyecto Angular
- `ng serve -o` se utiliza para iniciar el servidor de desarrollo y abrir automáticamente el navegador en la página principal del proyecto
- `ng g c components/nav` se utiliza para generar un componente llamado "nav" en la carpeta "components"
- `ng g s services/departamento` se utiliza para generar un servicio llamado "departamento" en la carpeta "services"

# Modulos vistos:
- `FormsModule` se utiliza para trabajar con formularios en Angular
- `RouterModule` se utiliza para establecer las rutas en una aplicación Angular
  

# Conceptos fundamentales de Angular

**Components**
Angular applications are built using reusable components. A component is a TypeScript class with an associated template that describes the component's layout and content. Here's an example of a simple component:

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-welcome',
  template: `<h1>Hello, {{ name }}</h1>`
})
export class WelcomeComponent {
  name = 'John';
}
```

**Templates**
Templates are used to describe the layout and content of a component. They are written in HTML and can include placeholders for data binding and directives for logic and control flow. Here's an example of a template:
```
<div>
  <h1>Welcome to Angular</h1>
  <p>This is a simple example</p>
</div>
```

**Data binding**
Data binding is used to keep the template and component class in sync. Angular supports several types of data binding, including property binding, event binding, and two-way binding. Here's an example of property binding:

```
<input [value]="name" (input)="name = $event.target.value">
```
**Services**
Services are used to share data and functionality across components. They are typically used for tasks such as fetching data from a server, logging, and validation. Here's an example of a service:

```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class MyService {
  getData() {
    return [1, 2, 3];
  }
}
```
**Dependency Injection**
Dependency injection is a design pattern used to manage dependencies between objects and components. Angular has a built-in dependency injection framework that allows you to easily share services and other objects across your application. Here's an example of injecting a service into a component:

```
import { Component } from '@angular/core';
import { MyService } from './my.service';

@Component({ ... })
export class MyComponent {
  constructor(private myService: MyService) {}
}
```

**Routing**
Routing allows you to define different URLs for different pages in your application. Angular has a built-in router that allows you to define routes, navigate between routes, and pass data between routes. Here's an example of routing configuration:

```
import { Routes, RouterModule } from '@angular/router';
import { HomeComponent } from './home.component';
import { AboutComponent } from './about.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

**Directives**
Directives are used to add logic and control flow to templates. Angular has several built-in directives, such as *ngFor, *ngIf, and ngStyle, and you can also create your own custom directives. Here's an example of using the *ngFor directive:

<code><li *ngFor="let item of items">{{ item }}</li></code>

**Pipes**
Pipes are used to transform data in templates. Angular has several built-in pipes, such as date, currency, and uppercase, and you can also create your own custom pipes. Here's an example of using the date pipe:

<code><span>{{ date | date:'medium' }}</span></code>

**Modules**
Modules are used to organize an Angular application into cohesive blocks of functionality. An application is divided into multiple modules and each module can have its own components, services, and other objects. Here's an example of creating a module:

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { MyComponent } from './my.component';
import { MyService } from './my.service';

@NgModule({
  imports: [CommonModule],
  declarations: [MyComponent],
  providers: [MyService],
  exports: [MyComponent]
})
export class MyModule { }
```

