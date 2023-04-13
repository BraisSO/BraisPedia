<span style="font-family: Times New Roman;">
<span style="text-align: justify">
<span style="font-size: medium;">

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

```typescript
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
```typescript
<div>
  <h1>Welcome to Angular</h1>
  <p>This is a simple example</p>
</div>
```

**Data binding**
Data binding is used to keep the template and component class in sync. Angular supports several types of data binding, including property binding, event binding, and two-way binding. Here's an example of property binding:

```typescript
<input [value]="name" (input)="name = $event.target.value">
```
**Services**
Services are used to share data and functionality across components. They are typically used for tasks such as fetching data from a server, logging, and validation. Here's an example of a service:

```typescript
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

```typescript
import { Component } from '@angular/core';
import { MyService } from './my.service';

@Component({ ... })
export class MyComponent {
  constructor(private myService: MyService) {}
}
```

**Routing**
Routing allows you to define different URLs for different pages in your application. Angular has a built-in router that allows you to define routes, navigate between routes, and pass data between routes. Here's an example of routing configuration:

```typescript
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

```typescript
<li *ngFor="let item of items">{{ item }}</li>
```

```typescript
<tr *ngFor="let key of keys">
            <td>{{key}}</td>
            <td *ngIf="extCurrenciesNames[key]!='';else elseBlock">{{extCurrenciesNames[key]}}</td>
            <ng-template #elseBlock>-</ng-template>
            <td>{{extCurrencies.eur[key]}}</td>
        </tr>
```

**Pipes**
Pipes are used to transform data in templates. Angular has several built-in pipes, such as date, currency, and uppercase, and you can also create your own custom pipes. Here's an example of using the date pipe:

<code><span>{{ date | date:'medium' }}</span></code>

**Modules**
Modules are used to organize an Angular application into cohesive blocks of functionality. An application is divided into multiple modules and each module can have its own components, services, and other objects. Here's an example of creating a module:

```typescript
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

# CRUD
### HTML

```html
<p>Gardar Banda</p>
<form (submit)="saveBanda()">
<input type="text" name="nombre" [(ngModel)]="banda.nombre">
<input type="submit">
</form>


<p>Gardar disco</p>
<form (submit)="saveDisco()">
<input type="text" name="nombre" [(ngModel)]="disco.nombre">
<input type="number" name="ano" [(ngModel)]="disco.ano">
<select name="seleccion" [(ngModel)]="seleccion">
    <option *ngFor="let banda of bandas" [value]="banda.id" >{{banda.nombre}}</option>
</select>
<input type="submit">
</form>


<p>Eliminar Banda</p>
<form >
<select name="seleccion" [(ngModel)]="seleccion">
    <option *ngFor="let banda of bandas" [value]="banda.id" >{{banda.nombre}}</option>
</select>

<button (click)="eliminarBanda(seleccion)">Eliminar</button>

</form>



<p>Eliminar Disco</p>
<form >
<select name="seleccionDisco" [(ngModel)]="seleccionDisco">
    <option *ngFor="let disco of discos" [value]="disco.id" >{{disco.nombre}}</option>
</select>

<button (click)="eliminarDisco(seleccionDisco)">Eliminar</button>

</form>


<p>Editar Nombre Banda</p>
<form (submit)="editarBanda(seleccion)">
    <input type="text" name="nombre" [(ngModel)]="banda.nombre">
    <select name="seleccion" [(ngModel)]="seleccion">
        <option *ngFor="let banda of bandas" [value]="banda.id">{{banda.nombre}}</option>
    </select>

<input type="submit">
</form>

<p>Editar Nombre Disco</p>
<form (submit)="editarDisco(seleccionDisco)">
    <input type="text" name="nombre" [(ngModel)]="disco.nombre">
    <select name="seleccionDisco" [(ngModel)]="seleccionDisco">
        <option *ngFor="let disco of discos" [value]="disco.id" >{{disco.nombre}}</option>
    </select>    

<input type="submit">
</form>

```


### Model
```typescript
export class Banda{
     id:number=0;
     nombre:string="";
    
}
```
### Service
```typescript
import { Injectable } from '@angular/core';
import {HttpClient,HttpHeaders} from '@angular/common/http'
import { Disco } from '../models/Disco';


@Injectable({
  providedIn: 'root'
})
export class DiscoService {

  URL:string="http://localhost:8080/api"
  httpHeaders:HttpHeaders=new HttpHeaders({'Content-Type' : 'application/json'});

  constructor(private http:HttpClient) { }


  guardarDisco(disco:Disco){
    return this.http.post<Disco>(this.URL+'/guardar-album',disco,{headers:this.httpHeaders});

  }

  listarDiscos(){
    return this.http.get<Disco>(this.URL+'/listar-albums');
  }

  
  eliminarDisco(id:number){
    return this.http.delete<Disco>(this.URL+'/eliminarAlbum-'+id);
  }

  editarDisco(id:number, disco:Disco){
    return this.http.put<Disco>(this.URL+'/modificarAlbum/'+id,disco,{headers:this.httpHeaders});
  }


}

```

### Component Typescript 

```typescript
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';
import { Banda } from 'src/app/models/Banda';
import { Disco } from 'src/app/models/Disco';
import { BandaService } from 'src/app/services/banda.service';
import { DiscoService } from 'src/app/services/disco.service';

@Component({
  selector: 'app-formulario',
  templateUrl: './formulario.component.html',
  styleUrls: ['./formulario.component.css']
})
export class FormularioComponent implements OnInit {

seleccion=0;
seleccionDisco=0;
bandas:any=[];
discos:any=[];

banda:Banda= new Banda();
disco:Disco= new Disco();

  constructor(private discoService:DiscoService, private bandaService:BandaService, private router:Router) { }

  ngOnInit(): void {
    this.listaBandas();
    this.listarDiscos();
  }

  listaBandas(){
    this.bandaService.listarBandas().subscribe(res=>{
      console.log(res)
      this.bandas=res;
    })
  }

  listarDiscos(){
    this.discoService.listarDiscos().subscribe(res=>{
      console.log(res);
      this.discos=res;
    })
  }

saveBanda(){
  this.bandaService.guardarBanda(this.banda).subscribe()
  
}

saveDisco(){
 this.disco.banda.id=(Number)(this.seleccion);
 this.discoService.guardarDisco(this.disco).subscribe(res=> console.log(res));
 
}

eliminarBanda(id:number){
  this.bandaService.eliminarBanda(id).subscribe(res=>{
    console.log(res)
  })
}

eliminarDisco(id:number){
  this.discoService.eliminarDisco(id).subscribe(res=>{
    console.log(res)
  })
}

editarBanda(id:number){
  this.bandaService.editarBanda(id, this.banda).subscribe(res=>{
    console.log(res);
  })
}

editarDisco(id:number){
  this.discoService.editarDisco(id, this.disco).subscribe(res=>{
    console.log(res);
  })
}

}

```

# Extra

**Como recargar una página tras ejecutar una funcion:**
<code> location.reload(); //recarga de la pagina </code>

Si queremos enviar a otra dirección tiene que ser con **<i>Router</i>**.

<hr>

**Como recorrer un mapa:**
```typescript
  getExtMoneyList(){
    this.extCurrencyService.getExtCurrencies().subscribe(res=>{
      console.log(res);
      this.extCurrencies=res;

      let i:number=0;
      for (const key in this.extCurrencies.eur) {
        this.keys[i]=key;
        i++;
        console.log(key, this.extCurrencies.eur[key]);
      }      
    })
  }

```
**Configurar Debugger**

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "ng debugging",
            "type": "chrome",
            "request": "launch",
            "url": "http://localhost:4200",
        },
        {
            "name": "ng serve",
            "type": "chrome",
            "request": "launch",
            "preLaunchTask": "npm: start",
            "url": "http://localhost:4200",
        },
        {
            "name": "ng test",
            "type": "chrome",
            "request": "launch",
            "preLaunchTask": "npm: test",
            "url": "http://localhost:9876/debug.html",
        }
    ]
}

```

Una vez puesto esto en la configuración, marcamos los breakpoint y al ejecutar el código que queremos debuggear, se parará en dichos puntos. *Si ejecutamos otro componente o parte de la página no funciona.*


