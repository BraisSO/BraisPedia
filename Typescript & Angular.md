<span style="font-family: Times New Roman;">
<span style="text-align: justify">
<span style="font-size: medium;">

#TypeScript & Angular

**Índice**
- [TypeScript](#typescript)
- [Angular](#angular)
- [Modulos:](#modulos)
- [Conceptos fundamentales de Angular](#conceptos-fundamentales-de-angular)
- [CRUD](#crud)
    - [HTML](#html)
    - [Model](#model)
    - [Service](#service)
    - [Component Typescript](#component-typescript)
- [Extra](#extra)
        - [Como recargar una página tras ejecutar una funcion:](#como-recargar-una-página-tras-ejecutar-una-funcion)
      - [Como recorrer un mapa:](#como-recorrer-un-mapa)
      - [Configurar Debugger](#configurar-debugger)
- [Multi-Idioma](#multi-idioma)
- [Testing con Jasmine](#testing-con-jasmine)
    - [Ejemplo propio](#ejemplo-propio)
    - [Enlaces de interés testing](#enlaces-de-interés-testing)
- [Interacción entre componentes](#interacción-entre-componentes)
- [Inyección de dependencias](#inyección-de-dependencias)
- [Asincronía en JavaScript/Typescript + Angular](#asincronía-en-javascripttypescript--angular)
- [Funciones importantes](#funciones-importantes)
    - [Map \& Filter](#map--filter)
- [Cambio de flujo y directivas Angular17](#cambio-de-flujo-y-directivas-angular17)
      - [@if @else](#if-else)
      - [@for](#for)
      - [@defer](#defer)
- [Enlaces de interés](#enlaces-de-interés)



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
- `ng g c features/demo --skip-import` *skip-import* indica al CLI que no importe automáticamente el componente en el archivo del módulo correspondiente, lo que significa que tendrás que hacerlo manualmente.
- `ng g module features/demo --routing` creacion del module y del routing module para cada componente
- `ng test --code-coverage` ejecución de los test

# Modulos:
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
En Angular, una pipe (tubería) es una característica que permite transformar datos antes de mostrarlos en la vista. Una pipe toma un valor de entrada y devuelve un valor transformado. Las pipes se utilizan para formatear, filtrar y transformar datos.

Aquí hay algunos ejemplos de pipes en Angular:

**DatePipe:** Esta pipe se utiliza para formatear fechas. Por ejemplo, si tenemos una fecha en formato de objeto Date y queremos mostrarla en la vista en un formato legible por humanos, podemos utilizar la DatePipe de la siguiente manera:

`<p>{{ fecha | date:'dd/MM/yyyy' }}</p>`

En este ejemplo, fecha es un objeto Date y la pipe date formatea la fecha en formato dd/MM/yyyy.

**UppercasePipe:** Esta pipe se utiliza para convertir texto en mayúsculas. Por ejemplo, si queremos mostrar un título en mayúsculas, podemos utilizar la UppercasePipe de la siguiente manera:

`<h1>{{ titulo | uppercase }}</h1>`

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

##### Como recargar una página tras ejecutar una funcion:
<code> location.reload(); //recarga de la pagina </code>

Si queremos enviar a otra dirección tiene que ser con **<i>Router</i>**.

<hr>

#### Como recorrer un mapa:
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
#### Configurar Debugger

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


# Multi-Idioma

Para crear una aplicación Angular multilingüe con ngx-translate, hay que seguir los siguientes pasos:

Instalar los paquetes de ngx-translate:

``npm i @ngx-translate/core --save``

``npm i @ngx-translate/http-loader --save``

Importar y registrar el módulo TranslateModule en app.module.ts:

```typescript
import { TranslateLoader, TranslateModule } from '@ngx-translate/core';
import { TranslateHttpLoader } from '@ngx-translate/http-loader';
import { HttpClient, HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [
    BrowserModule,
    HttpClientModule,
    TranslateModule.forRoot({
      loader: {
        provide: TranslateLoader,
        useFactory: httpTranslateLoader,
        deps: [HttpClient]
      }
    })
  ],
  bootstrap: [AppComponent]
})

export class AppModule { }

export function httpTranslateLoader(http: HttpClient) {
  return new TranslateHttpLoader(http);
}

```

Crear los archivos de traducción en la carpeta assets/i18n para cada idioma y configurar el loader. Por ejemplo, para el inglés y el español:

`src/assets/i18n/en.json`
`src/assets/i18n/es.json`


En estos archivos se definen los valores en pares clave-valor.

Implementar las traducciones en el componente con el servicio TranslateService:

```typescript
import { TranslateService } from '@ngx-translate/core';

export class AppComponent {
  constructor(
    public translate: TranslateService
  ) {
    translate.addLangs(['en', 'es']);
    translate.setDefaultLang('en');
  }
}
```
Con translate.addLangs(['en', 'es']) se define qué idiomas se van a utilizar y con translate.setDefaultLang('en') se establece el idioma por defecto.

Añadir un selector de idiomas en la interfaz de usuario que llame a la función translate.use(lang) para cambiar el idioma de la aplicación:

```typescript
switchLang(lang: string) {
  this.translate.use(lang);
}
```

```html
<div class="form-inline">
  <select 
      class="form-control" 
      #selectedLang 
      (change)="switchLang(selectedLang.value)">
    <option *ngFor="let language of translate.getLangs()" 
      [value]="language"
      [selected]="language === translate.currentLang">
      {{ language }}
    </option>
  </select>
  </div>

```
Este selector permitirá cambiar el idioma de la aplicación.

Como luciría el **HTML**:
```html
<nav class="navbar navbar-dark bg-primary">
  <div class="container">
    <a class="navbar-brand">
      {{'Sitetitle' | translate }}
    </a>
    <div class="form-inline">
      <select class="form-control" #selectedLang (change)="switchLang(selectedLang.value)">
        <option *ngFor="let language of translate.getLangs()" [value]="language"
          [selected]="language === translate.currentLang">
          {{ language }}
        </option>
      </select>
    </div>
  </div>
</nav>
<div class="container">
  <form>
    <div class="form-group">
      <label>{{'Name' | translate}}</label>
      <input type="text" class="form-control">
      <small class="text-danger">{{'NameError' | translate}}</small>
    </div>
    <div class="form-group">
      <label>{{'Email' | translate}}</label>
      <input type="email" class="form-control">
    </div>
    <div class="form-group">
      <label>{{'PhoneNo' | translate}}</label>
      <input type="tel" class="form-control">
    </div>
    <div class="form-group">
      <label>{{'Password' | translate}}</label>
      <input type="password" class="form-control">
    </div>
    <div class="form-group">
      <label>{{'Bio' | translate}}</label>
      <textarea rows="3" class="form-control"></textarea>
    </div>
    <div class="form-group form-check">
      <input type="checkbox" class="form-check-input">
      <label class="form-check-label">{{'TermsConditions' | translate}}</label>
    </div>
    <button type="submit" class="btn btn-block btn-danger">{{'Submit' | translate}}</button>
  </form>
</div>
```


Como luciría el **app.component.ts**:

```typescript
import { Component } from '@angular/core';
import { TranslateService } from '@ngx-translate/core';
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
export class AppComponent {
  constructor(public translate: TranslateService) {
    translate.addLangs(['en', 'nl']);
    translate.setDefaultLang('en');
  }
  switchLang(lang: string) {
    this.translate.use(lang);
  }
}
```

# Testing con Jasmine

### Ejemplo propio

 `ng test --code-coverage` ejecución de los test.
 `ng test --include [src/app/x/.component.spec.ts] --code-coverage` ejecución solo del fichero de test deseado

 Conceptos en los test cuando vemos el *coverage*:

 - **Statements**: se refiere al porcentaje de sentencias de código (declaraciones de variables, asignaciones, llamadas a funciones, etc.) que han sido ejecutadas al menos una vez por los tests. 

- **Branches**: se refiere al porcentaje de caminos posibles en el código que han sido ejecutados al menos una vez por los tests. Un "camino" se define como un flujo de ejecución dentro de una estructura de control de flujo, como un if, un while, un for, etc. 

- **Functions**: se refiere al porcentaje de funciones que han sido ejecutadas al menos una vez por los tests. 

- **Lines**: se refiere al porcentaje de líneas de código que han sido ejecutadas al menos una vez por los tests.

```typescript
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { HttpClientTestingModule } from '@angular/common/http/testing';

import { UserEditComponent } from './user-edit.component';
import { RouterTestingModule } from '@angular/router/testing';
import { FormControl, ReactiveFormsModule } from '@angular/forms';
import { MaterialModule } from 'src/app/material.module';
import { TranslateModule } from '@ngx-translate/core';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';

describe('UserEditComponent', () => {
  let component: UserEditComponent;
  let fixture: ComponentFixture<UserEditComponent>;
  let control: FormControl;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [HttpClientTestingModule,
        RouterTestingModule,
        ReactiveFormsModule,
        MaterialModule,
        TranslateModule.forRoot(),
        BrowserAnimationsModule

      ],

      declarations: [UserEditComponent]
    })
      .compileComponents();

    fixture = TestBed.createComponent(UserEditComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  beforeEach(() => {
    control = new FormControl();
  });

    it('should return true if username exists in otherUsersUsername array', () => {
        component.otherUsersUsername = ['john', 'jane', 'smith'];
        expect(component.checkUsername('jane')).toBe(true);
      });

    it('should create', () => {
      expect(component).toBeTruthy();
    });


    it('should return false if username does not exist in otherUsersUsername array', () => {
      component.otherUsersUsername = ['john', 'jane', 'smith'];
      expect(component.checkUsername('doe')).toBe(false);
    });

    it('should return null if the email is valid', () => {
      control.setValue('example@test.com');
      const validatorFn = component.emailValidator();
      const result = validatorFn(control);
      expect(result).toBeNull();
    });

    it('should return an error object if the email is invalid', () => {
      control.setValue('example@test.');
      const validatorFn = component.emailValidator();
      const result = validatorFn(control);
      expect(result).toEqual({ emailInvalid: true });
    });

    it('should return null if the control value is empty', () => {
      control.setValue('');
      const validatorFn = component.emailValidator();
      const result = validatorFn(control);
      expect(result).toBeNull();
    });
  })

```

### Enlaces de interés testing
- [Guía Jasmine](https://mbascoy.github.io/knowledge/Programacion/Angular/Conceptos%20avanzados/Testing%20con%20Jasmine.html)
  
- [Elementos básicos (beforeEach(), byCss(), ejemplos, etc.)](https://angular.io/guide/testing-components-basics)
  
- [Testing component escenarios](https://angular.io/guide/testing-components-scenarios#component-testing-scenarios)

- Videos:
  - [HttpTesting & Pipe Testing](https://www.youtube.com/playlist?list=PL_WGMLcL4jzVoCpd-QhvfM0v8lNUa8OSQ)
  - [Testing con interactividad (asignar a inputs + click)](https://www.youtube.com/watch?v=c3ahq1x_OOo&list=PLHgpVrCyLWApcgDNVxsdCDXy3p3XmxwbX&index=3&pp=iAQB)
  - [Tipos de *expect*](https://www.youtube.com/watch?v=idLGQYEv2Ts&list=PLHgpVrCyLWApcgDNVxsdCDXy3p3XmxwbX&index=5&pp=iAQB)
    

# Interacción entre componentes
- [Interacción entre componentes (@Input, @Output)](https://docs.angular.lat/guide/component-interaction#component-interaction)
  
# Inyección de dependencias

Cuando creamos código que va a ser inyectado en un componente a través de un servicio, **debemos registrarlo en los providers del módulo correspondiente al componente**. Se registra en el array de ***providers***.

Otra forma de hacer esta inyección es en el propio servicio, agregando despues de las importaciones: 

```typescript
@Injectable({
  providedIn: 'root'
})
```


Para inyectar un servicio dentro de otro lo hacemos de la siguiente manera:

![Inyección Angular](./Images/inyecciónAngular.png)

# Asincronía en JavaScript/Typescript + Angular
- [Asincronía Manz.dev](https://lenguajejs.com/javascript/asincronia/que-es/)

**Async-Await**

![Async](Images/async.PNG)

**Array Promises**

![Async](Images/ArrayPromise.PNG)
![Async](Images/array-all.PNG)


**Angular async & http**

En Angular, puedes usar async y await en funciones para manejar llamadas asincrónicas de manera más limpia y legible. Esto es especialmente útil cuando trabajas con operaciones asíncronas, como solicitudes HTTP, operaciones de lectura/escritura en bases de datos, o cualquier otro proceso que tome tiempo en completarse. Aquí te explico cómo funciona:

Palabra clave async: Puedes declarar una función como async anteponiendo la palabra clave async antes de la palabra clave function. Por ejemplo:

```typescript
async miFuncion() {
  // Código asincrónico
}
```
Palabra clave await: Dentro de una función async, puedes usar la palabra clave await antes de una expresión que devuelve una promesa. Esto pausará la ejecución de la función hasta que la promesa se resuelva o se rechace. Por ejemplo:

```typescript
async obtenerDatos() {
  const resultado = await this.servicio.obtenerDatos();
  // El código aquí se ejecutará después de que la promesa se resuelva
  console.log(resultado);
}
```
En este ejemplo, this.servicio.obtenerDatos() es una función que devuelve una promesa. Usar await en esta llamada permite que el código espere hasta que la promesa se resuelva antes de continuar.

Manejo de errores: Puedes usar try...catch para manejar errores cuando se utiliza await. Si la promesa se rechaza, se generará una excepción que puedes capturar y manejar de la siguiente manera:

```typescript
async obtenerDatos() {
  try {
    const resultado = await this.servicio.obtenerDatos();
    console.log(resultado);
  } catch (error) {
    console.error('Error al obtener datos:', error);
  }
}
```
Esto asegura que los errores se manejen de manera adecuada y no rompan la ejecución del programa.

Retorno de valores asincrónicos: Puedes retornar valores desde funciones async, y estos valores se envolverán automáticamente en una promesa resuelta. Por ejemplo:

```typescript
async obtenerDatos() {
  const resultado = await this.servicio.obtenerDatos();
  return resultado;
}
```
Si llamas a esta función, obtendrás una promesa que se resolverá con el valor de resultado.

Uso en plantillas: En las plantillas de Angular, puedes usar el operador async en las directivas *ngIf y *ngFor para manejar la visualización condicional o la iteración sobre datos asincrónicos.
Por ejemplo, puedes usar *ngIf con una función async en tu plantilla de la siguiente manera:

```typescript
<div *ngIf="condicionAsincronica()"></div>
```

En resumen, el uso de async y await en funciones en Angular simplifica el manejo de operaciones asincrónicas, haciendo que tu código sea más limpio y legible al evitar el anidamiento excesivo de callbacks y promesas.

Pero que retorna un await, una promesa, un objeto?

await retorna el resultado de una promesa. Cuando utilizas await en una función async, estás pausando la ejecución de esa función hasta que la promesa se resuelva o se rechace. El resultado de esa promesa se devuelve como valor. Si la promesa se resuelve exitosamente, el valor de retorno será el valor resuelto de la promesa. Si la promesa es rechazada (por ejemplo, debido a un error), se lanzará una excepción.

En resumen:

Si la promesa se resuelve correctamente, await retorna el valor resuelto de la promesa.
Si la promesa es rechazada, await genera una excepción que puedes manejar usando un bloque try...catch o permitir que se propague como una excepción no capturada.
Aquí tienes un ejemplo para ilustrar esto:

```typescript
async function ejemploAsync() {
  try {
    const resultado = await promesaAsincrona();
    console.log("La promesa se resolvió:", resultado);
  } catch (error) {
    console.error("La promesa se rechazó:", error);
  }
}

function promesaAsincrona() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      // Simular una promesa que se resuelve después de un tiempo
      resolve("¡Éxito!");
      // O puedes rechazar la promesa: reject(new Error("¡Error!"));
    }, 1000);
  });
}

ejemploAsync();
```
En este ejemplo, await promesaAsincrona() espera a que la promesa se resuelva y luego retorna el valor resuelto, que en este caso es la cadena "¡Éxito!".

**Y en que casos se devuelve un objeto promesa en el que tenemos que utilizar then, cuando no se usa async y await?**

Las promesas se utilizan comúnmente en JavaScript para manejar operaciones asíncronas. Puedes encontrarte con promesas en varios casos, incluso cuando no estás utilizando async y await. Aquí hay algunos ejemplos de situaciones en las que obtendrás una promesa y necesitarás usar el método .then() para manejar la resolución de esa promesa:

Solicitudes HTTP: Cuando haces una solicitud HTTP utilizando Angular's HttpClient (o cualquier otra librería similar), obtendrás una promesa como resultado. Aquí hay un ejemplo sin async y await:

```typescript
this.httpClient.get('https://api.example.com/data')
  .then(response => {
    // Manejar la respuesta
    console.log(response);
  })
  .catch(error => {
    // Manejar el error
    console.error(error);
  });
```
Operaciones de lectura/escritura en bases de datos: Al interactuar con bases de datos o sistemas de almacenamiento, a menudo se devuelven promesas para representar la finalización de la operación. Esto puede ocurrir en bibliotecas como Firebase o IndexedDB.

```typescript
myDatabase.readData().then(data => {
  // Hacer algo con los datos leídos
}).catch(error => {
  // Manejar el error
});
```

Carga de archivos: Cuando cargas un archivo en una aplicación web, la operación de carga se realiza de forma asíncrona y suele devolver una promesa.

```typescript
uploadFile(file)
  .then(response => {
    // Manejar la respuesta de la carga
  })
  .catch(error => {
    // Manejar errores de carga
  });
  ```

Operaciones de temporización: Al usar funciones como setTimeout o setInterval, no se utiliza async y await, pero aún así obtienes una promesa que puedes usar para manejar la finalización de la temporización.

```typescript
const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));

delay(1000)
  .then(() => {
    // Este código se ejecuta después de 1 segundo
  });
  ```

En estos casos, aunque no estés utilizando async y await, aún trabajas con promesas y debes usar .then() y .catch() para manejar la resolución y el manejo de errores de esas promesas. La elección de utilizar async y await o métodos .then() depende de tu preferencia y del estilo de programación que desees seguir.

**Y dame un ejemplo de peticiones http pero esta vez con async y await**

Por supuesto, aquí tienes un ejemplo de cómo hacer una solicitud HTTP utilizando async y await en Angular con HttpClient:

```typescript
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-ejemplo-http',
  templateUrl: './ejemplo-http.component.html',
  styleUrls: ['./ejemplo-http.component.css']
})
export class EjemploHttpComponent implements OnInit {
  responseData: any;

  constructor(private http: HttpClient) { }

  ngOnInit(): void {
    this.obtenerDatos();
  }

  async obtenerDatos() {
    try {
      this.responseData = await this.http.get('https://api.example.com/data').toPromise() 
      //toPromise() está deprecates usar lastValueFrom() o firsValueFrom();
      //import { lastValueFrom, firstValueFrom } from 'rxjs';
      console.log('Datos obtenidos:', this.responseData);
    } catch (error) {
      console.error('Error al obtener datos:', error);
    }
  }
}
```
En este ejemplo:

Importamos HttpClient desde @angular/common/http.

Creamos un método obtenerDatos que está marcado como async. Esto nos permite usar await dentro de la función.

Dentro de obtenerDatos, utilizamos await en this.http.get('https://api.example.com/data').toPromise() para hacer una solicitud GET a la URL proporcionada. toPromise() convierte la observación en una promesa que podemos esperar con await.

Utilizamos un bloque try...catch para manejar posibles errores. Si la solicitud se resuelve correctamente, el resultado se almacena en this.responseData. Si ocurre un error, se captura y se maneja en el bloque catch.

Este es un ejemplo básico de cómo puedes utilizar async y await con HttpClient en Angular para realizar solicitudes HTTP de manera asíncrona y manejar los resultados de una manera más legible y ordenada.


**Asincronía bloqueante vs no bloqueante (bloquente == async await)**

![bloqueantevsnobloqueante](Images/bloqueantevsnobloqueante.PNG)

**Ejemplo simple de como consumir la asincronía (en este caso se convierte un observable en una promesa con lastValueFrom()**
![Async-Await](Images/async-await.PNG)

**Consumo directo de una promesa bloqueante async/await**

![Async-Await](Images/asyncawaitPromise.PNG)

**Consumo de una promesa no bloqueante .then & .catch**

![Async-Await](Images/promiseThenCatch.PNG)


**Como realizar peticiones http anidadas**

```typescript
ngOnInit(){
  this.tabsService.getUsers()
    .pipe(
      switchMap(data1 => {
        console.log("Data from first request: ", data1);
        return this.tabsService.getUsers2();
      }),
      switchMap(data2 => {
        console.log("Data from second request: ", data2);
        return this.tabsService.getUsers3();
      })
    )
    .subscribe({
      next: data3 => {
        console.log("Data from third request: ", data3);
      },
      error: error => console.log(error.status)
    });
}
```

# Funciones importantes
### Map & Filter
- La función **map** sirve para devolver un array u objeto con las características que te interesan de otro objeto con más propiedades.
- La función **filter** devuelve un array con los objetos o propiedades del objeto que cumplan una condición especificada.

En gran medida estas dos funciones nos permiten iterar sobre un array u objeto evitando usar bucles.

**Basico**

![MapYFilter](Images/mapYfilter.png)

**Map & Filter combinados con tipado retornando un array**

![MapYFilterTipados](Images/map&filterTipados.png)

**Map & Filter combinados con tipado retornando un objeto**

![MapYFilterTipadosObjetos](Images/map&filterTipadosObjetos.png)

# Cambio de flujo y directivas Angular17

#### @if @else
 ![ngif](Images/ngif.jpeg)

#### @for
 ![for](Images/ngfor.jpeg)
 
#### @defer
![defer](Images/defer.jpeg)
 



# Enlaces de interés
- [Curso muy completo de angular](https://www.youtube.com/playlist?list=PLU8oAlHdN5BnNAe8zXnuBNzKID39DUwcO)
- [Angular Sinals - Nueva funcionalidad de Angular para el control de estados](https://angular.io/guide/signals)
  








