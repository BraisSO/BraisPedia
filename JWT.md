<span style="font-family: Times New Roman;">
<span style="text-align: justify">
<span style="font-size: medium;">

# JWT - Flask

JWT son las siglas de JSON Web Tokens, un estándar abierto que define un formato compacto y seguro para transmitir información entre dos partes como un objeto JSON.

Los JWT se utilizan comúnmente para la autenticación y la autorización en aplicaciones web y móviles, ya que proporcionan una forma segura y fácil de transmitir información de usuario entre el servidor y el cliente sin la necesidad de almacenar esa información en una base de datos.

**Paso 1: Instala las dependencias**
Para usar JWT con Python y Flask, necesitarás instalar las siguientes dependencias:

```bash
flask
flask_jwt_extended
```
Puedes instalarlas usando pip:
```bash
pip install flask
pip install flask_jwt_extended
```

**Paso 2: Configura Flask**
Crea una instancia de la aplicación Flask y configura una clave secreta para usar con JWT. Puedes hacer esto de la siguiente manera:

```python
from flask import Flask
from flask_jwt_extended import JWTManager

app = Flask(__name__)
app.config['JWT_SECRET_KEY'] = 'super-secret' # Reemplaza con tu propia clave secreta
jwt = JWTManager(app)
```

**Paso 3: Crea una ruta de login**
Crea una ruta de login donde los usuarios puedan enviar sus credenciales para autenticarse. En esta ruta, verificarás las credenciales y generarás un JWT si la autenticación es exitosa. Puedes hacer esto de la siguiente manera:

```python
from flask import request, jsonify
from flask_jwt_extended import create_access_token

@app.route('/login', methods=['POST'])
def login():
    username = request.json.get('username', None)
    password = request.json.get('password', None)

    # Verifica las credenciales del usuario aquí
    # ...

    # Si las credenciales son correctas, genera un JWT y devuelve una respuesta al cliente
    access_token = create_access_token(identity=username)
    return jsonify(access_token=access_token), 200

```

**Paso 4: Crea una ruta protegida**
Crea una ruta protegida que solo pueda ser accedida por usuarios autenticados con un JWT válido. Puedes hacer esto de la siguiente manera:

```python
from flask_jwt_extended import jwt_required, get_jwt_identity

@app.route('/protected', methods=['GET'])
@jwt_required()
def protected():
    current_user = get_jwt_identity()
    return jsonify(logged_in_as=current_user), 200
```

**Paso 5: Ejecuta la aplicación**
Ejecuta la aplicación Flask y realiza una solicitud a la ruta de login para obtener un JWT. Luego, usa el JWT para acceder a la ruta protegida. Puedes hacer esto usando una herramienta como curl o Postman.

Para ello, agrega una cabecera HTTP llamada "Authorization" a la solicitud y establece su valor en "Bearer [TOKEN_JWT]", donde [TOKEN_JWT] es el token JWT que recibiste en la respuesta. Por ejemplo:

```bash
Authorization: Bearer eyJhbGciOiJIUzI1Ni...
```

Con estos pasos, podrás probar la ruta de login en Flask usando Postman y ver cómo funciona la autenticación JWT.



<hr>

# Primera aproximación tokens JWT Back-Front 

## Flask

```python
from flask import Flask,jsonify
from flask_jwt_extended import create_access_token, JWTManager
import os
from dotenv import load_dotenv
import datetime
from flask_cors import CORS

load_dotenv()

app = Flask(__name__)

#CORS -config
app.config['JSON_AS_ASCII'] = False
CORS(app)

#jwt-config
app.config['JWT_SECRET_KEY'] = os.getenv('SECRET_KEY')
jwt=JWTManager(app)

def obtener_payload():
    payload = {
        'user': 'pepe',
        'rol' : 'admin'
    }
    return payload

@app.post("/token")
def crear_token():
    token_config ={
        'payload': obtener_payload(),
        'exp': datetime.datetime.utcnow() + datetime.timedelta(hours=1)
    }
    
    token = create_access_token(token_config)
    return jsonify({"token": token})


if __name__=='__main__':
    app.run(debug=True)

```
 La aplicación define una única ruta /token a la que se puede acceder mediante un método HTTP POST.

Cuando se accede a la ruta /token, se genera un token JWT (JSON Web Token) utilizando la biblioteca flask_jwt_extended. El token contiene información de usuario en forma de payload, que se obtiene a través de la función obtener_payload(). En este caso, el payload contiene el nombre de usuario y el rol.

El token JWT también tiene una fecha de expiración, que se establece en una hora después de la hora actual utilizando la biblioteca datetime. Finalmente, el token se devuelve como una respuesta JSON a través de la función jsonify().

Además, la aplicación utiliza las bibliotecas dotenv y os para cargar variables de entorno desde un archivo .env, lo que permite ocultar información sensible como la clave secreta JWT.

También se utiliza la biblioteca flask_cors para permitir solicitudes CORS (Cross-Origin Resource Sharing) desde diferentes dominios y evitar problemas de seguridad en el navegador web.


## JS - Front

```javascript
    <script>
        fetch('http://localhost:5000/token')
        .then(response=> response.json())
        .then(json=> {
            sessionStorage.setItem('token', String(json.token));
        })
    </script>
```

Al cargar la página, el script realiza una solicitud a la URL de Flask utilizando la función fetch().

La respuesta de la solicitud es un objeto JSON, este se convierte a un objeto JavaScript utilizando la función `response.json()`. Después de eso, el token se guarda en la sesión de almacenamiento local del navegador web utilizando la función `sessionStorage.setItem()`.

El token se convierte a una cadena utilizando la función String() y a su vez se guarda en la sesión de almacenamiento local con el nombre token. Esto permite que el token esté disponible en otras páginas del mismo sitio web en la misma sesión del navegador.


## Proteger Rutas

*Ejemplo más ampliado y mejor explicado en la parte JWT de  [Flask](https://github.com/BraisSO/BraisPedia/blob/main/Python%26Flask.md).*

```python
from flask import Flask,jsonify, request, make_response
from flask_jwt_extended import create_access_token, JWTManager, jwt_required
import os
from dotenv import load_dotenv
import datetime
from flask_cors import CORS

load_dotenv()

app = Flask(__name__)

#CORS -config
app.config['JSON_AS_ASCII'] = False
CORS(app)

#jwt-config
app.config['JWT_SECRET_KEY'] = os.getenv('SECRET_KEY')
jwt=JWTManager(app)

def obtener_payload():
    payload = {
        'user': 'pepe',
        'rol' : 'admin'
    }
    return payload

@app.post("/token")
def crear_token():
    token_config ={
        'payload': obtener_payload(),
        'exp': datetime.datetime.utcnow() + datetime.timedelta(hours=1)
    }
    
    token = create_access_token(token_config)
    return jsonify({"token": token})


@app.post("/login")
def login():
    username= request.json['username']
    password= request.json['password']
    
    if username == 'pepe' and password == 'abc123.':
        return crear_token()
    else:
        respuesta = make_response(({"error 401": "Error de autenticación"}))
        respuesta.status_code = 401
        return respuesta


@app.get("/admin")
@jwt_required()
def dashboard():
    return "Este es el panel del administrador y para poder acceder voy a necesitar enviar un token por la cabecera de autenticación"

    
@app.get("/dashboard")
@jwt_required()
def dashboard():
    users={
        'username': 'pepe',
        'email': 'pepe@es.es'
    }
    return jsonify(users)


if __name__=='__main__':
    app.run(debug=True)

```



Se han agregado nuevas rutas a la aplicación: /login y /admin.

La nueva ruta /login maneja las solicitudes de inicio de sesión y devuelve un token JWT si el nombre de usuario y la contraseña proporcionados son correctos. Si las credenciales son incorrectas, devuelve una respuesta con código de estado 401 (no autorizado).

La nueva ruta /admin utiliza el decorador `@jwt_required()` para restringir el acceso a usuarios autenticados solamente. En otras palabras, solo se puede acceder a esta ruta si se proporciona un token JWT válido en la cabecera de autenticación de la solicitud.

La función request ha sido importada para manejar las solicitudes HTTP entrantes y extraer los datos del cuerpo de la solicitud (como el nombre de usuario y la contraseña). También se ha importado la función `make_response` para generar respuestas personalizadas.

<hr>


# JWT Front Angular

*Aquí se documentarán los nuevos pasos a seguir. Muchos son similares o idénticos a proyectos previos de Angular, por lo que peuden estar obviados, para más información consultar directamente el proyecto `C:\Projects\Academy\Front-Back-Apps\App5FlaskAngularToken` o la documentación de Angular*

### Introducción

En la configuración inicial del proyecto que Angular decimos que sí al modulo de *routing*.

### Estructura de carpetas del proyecto

![EstructuraJWTAngular1](./Images/EstructuraJWTAngular1.PNG)

![EstructuraJWTAngular2](./Images/EstructuraJWTAngular2.PNG)


### Configuraciones globales

##<code>**app-routing.module.ts**</code>

En este fichero realizaremos la configuración de las rutas a las que se accederá desde nuestra app y asignaremos las protecciones que tengamos definidas para las mismas. Para que este fichero se cree automaticamente debemos darle a *si* en la creación del proyecto cuando nos pregunta si queremos routing en nuestro proyecto.

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { LoginComponent } from './components/login/login.component';
import { DashboardComponent } from './components/dashboard/dashboard.component';
import { DashboardGuard } from './guards/dashboard.guard';

const routes: Routes = [
    {path:'login', component: LoginComponent},
    {path: 'dashboard', component: DashboardComponent, canActivate: [DashboardGuard]} //Protección de la ruta que se añade prácticamente al final.
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }

```

## <code>**app-module.ts**</code>

Este es el archivo de modulos e importaciones del proyecto. En el se importan y declaran todos los componentes, servicios, directivas y pipes utilizados en la aplicación.

 Los proveedores de servicios que se utilizarán se definen en la propiedad ``providers``, donde en este caso se está utilizando el proveedor ``HTTP_INTERCEPTORS`` para agregar el interceptor personalizado ``TokenInterceptor``.
 
 El objeto en sí mismo tiene tres propiedades:

**``provide``**: indica qué servicio se está proporcionando. En este caso, el servicio es HTTP_INTERCEPTORS.
**``useClass``**: indica qué clase se utilizará para proporcionar el servicio. En este caso, se está utilizando la clase TokenInterceptor que se define en otro archivo del proyecto.
**``multi``**: indica si el proveedor se proporciona en un formato de matriz (true) o en un formato único (false). En este caso, se establece en true porque el interceptor es solo uno de muchos proveedores que pueden estar presentes en la aplicación.

En resumen, el objeto dentro de providers se utiliza para proporcionar un interceptor personalizado que agrega un token de autenticación a las solicitudes HTTP realizadas por la aplicación. Esto ayuda a asegurar que solo los usuarios autenticados puedan acceder a los recursos protegidos en la aplicación.

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { LoginComponent } from './components/login/login.component';
import { DashboardComponent } from './components/dashboard/dashboard.component';
import { ReactiveFormsModule } from '@angular/forms';
import { HTTP_INTERCEPTORS, HttpClientModule } from '@angular/common/http';
import { TokenInterceptor } from './interceptors/token.interceptor';

@NgModule({
  declarations: [
    AppComponent,
    LoginComponent,
    DashboardComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    ReactiveFormsModule,
    HttpClientModule
  ],
  providers: [
    {provide: HTTP_INTERCEPTORS, useClass:TokenInterceptor, multi: true} 
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }

```



# Interceptor

`ng g interceptor interceptors/token`

Un **interceptor** en Angular es un servicio que se utiliza para interceptar y modificar las solicitudes HTTP y las respuestas antes de que lleguen al servidor o se procesen en la aplicación cliente.

Los interceptores se definen en Angular mediante la creación de una clase que implementa la interfaz HttpInterceptor. Cada interceptor puede ser utilizado para agregar encabezados personalizados, modificar el cuerpo de la solicitud o la respuesta, y agregar información adicional a la solicitud o respuesta.

Los interceptores son útiles para agregar funcionalidades generales a las solicitudes HTTP en una aplicación Angular, como la adición de tokens de autorización a las solicitudes, la manipulación de encabezados de respuesta, la gestión de errores comunes y la gestión de solicitudes en caché.

Además, los interceptores pueden ser encadenados para ejecutarse en orden, lo que permite que varios interceptores modifiquen una solicitud o una respuesta antes de que se procesen.

## <code>**token.interceptor.ts**</code>

```typescript
import { Injectable } from '@angular/core';
import {
  HttpRequest,
  HttpHandler,
  HttpEvent,
  HttpInterceptor
} from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class TokenInterceptor implements HttpInterceptor {

  constructor() { }

  intercept(request: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {

    let interceptorRequest = request 
    let token = sessionStorage.getItem('token') 

    if (token != null) {
      interceptorRequest = request.clone({ 
        headers: request.headers.set('Authorization', 'Bearer ' + token)
      })
    }
    return next.handle(interceptorRequest);
  }
}

```
Dentro de la función intercept, el código primero recupera el token de autenticación almacenado en el sessionStorage. Si el token existe, se clona la solicitud HTTP original y se le agrega una cabecera de autenticación en la nueva solicitud clonada.

El objeto interceptorRequest es una instancia de ``HttpRequest<any>`` que se inicializa con la solicitud HTTP original. Si el token existe, se clona la solicitud original utilizando el método clone() y se le agrega una cabecera de autorización utilizando el método ``set()``.

La cabecera de autorización contiene el token de autenticación en formato ``"Bearer [token]"``. Esto es necesario para que el servidor pueda verificar la identidad del usuario que realiza la solicitud.

Finalmente, la función intercept devuelve el método ``handle()`` de la instancia de HttpHandler con la nueva solicitud interceptorRequest. Esto envía la solicitud modificada al siguiente controlador de solicitudes en la cadena.


# Guard

`ng g guard guards/dashboard`

Un **guard** en Angular es una clase que se utiliza para proteger las rutas de acceso en una aplicación. En otras palabras, los guards son utilizados para restringir el acceso a determinadas rutas basándose en ciertas condiciones.

Hay dos tipos de guards en Angular: los `canActivate` guards y los `canActivateChild` guards. Ambos guards son implementados en clases que deben implementar las interfaces `CanActivate` o `CanActivateChild`.

Los ``canActivate`` guards son utilizados para proteger rutas principales y verifican si un usuario puede acceder a una ruta determinada en función de ciertas condiciones, como la autenticación o los permisos de usuario.

Por otro lado, los `canActivateChild` guards son utilizados para proteger las rutas secundarias, que son rutas anidadas dentro de rutas principales, y también verifican si un usuario puede acceder a una ruta determinada basándose en ciertas condiciones.



## <code>**dashboard.guard.ts**</code>

```typescript
import { Injectable } from '@angular/core';
import { ActivatedRouteSnapshot, Router, RouterStateSnapshot, UrlTree } from '@angular/router';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class DashboardGuard {

  constructor(private router:Router){

  }

  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot): boolean {

    let token = sessionStorage.getItem('token')

    if(!token){
      this.router.navigate(['login'])
      return false
    }

    else{
      return true
    }

  }
  
}

```

En el código proporcionado, se define una clase ``DashboardGuard`` que implementa el guard ``canActivate``. Los guards son una forma de controlar el acceso a las rutas de la aplicación y asegurarse de que el usuario esté autenticado y autorizado para acceder a ellas.

La función ``canActivate`` es el método clave en este guard. Recibe dos argumentos:

**``route``** es una instancia de ``ActivatedRouteSnapshot``, que representa la ruta que se está intentando acceder.
**``state``** es una instancia de ``RouterStateSnapshot``, que representa el estado actual del enrutador.

Dentro de la función ``canActivate``, el código primero recupera el token de autenticación almacenado en el sessionStorage. Si el token no existe, el usuario no está autenticado y se redirige a la página de inicio de sesión utilizando el servicio Router. La función ``canActivate`` devuelve false, lo que significa que el acceso a la ruta se deniega.


# Modelos

## <code>**UserLogin.ts**</code>

```typescript
export class UserLogin {
    username:string
    password:string
    token:string = ""

constructor(username:string, password: string){
    this.username=username;
    this.password=password;
    }
}
```
El propósito de la propiedad **token** dentro del modelo, es almacenar el token de autenticación devuelto por el servidor después de que se haya realizado una solicitud de inicio de sesión exitosa. El token se utiliza posteriormente para autenticar al usuario en solicitudes posteriores al servidor.

## <code>**Dashboard.ts**</code>

```typescript
export class UserDashboard {
    username:string = ""
    email:string = ""
}
```


# Servicios

Aquí se realiza la lógica de negocio que van a tener los componentes de nuestra aplicación y se le pasán al propio componente que ejecutará las funciones.

## <code>**login.service.ts**</code>

```typescript
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs'
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { UserLogin } from '../model/UserLogin';

@Injectable({
  providedIn: 'root'
})
export class LoginService {

  URL:string = "http://localhost:5000"
  httpHeaders:HttpHeaders = new HttpHeaders ({'Content-Type': 'application/json'})

  constructor(private http: HttpClient) { }

  login(userLogin: UserLogin) : Observable<UserLogin> {
    return this.http.post<UserLogin>(this.URL+'/login', userLogin, {headers:this.httpHeaders})
  }
}

```

En la funcion `login` se verifican las credenciales del usuario contra el servidor y este devolverá el token.

## <code>**dashboard.service.ts**</code>

```typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';
import { UserDashboard } from '../model/Dashboard';


@Injectable({
  providedIn: 'root'
})
export class DashboardService {

  URL:string = "http://localhost:5000"

  constructor(private http:HttpClient) { }

getUsers():Observable<UserDashboard>{
  return this.http.get<UserDashboard>(this.URL+'/dashboard')
    }
}

```
El método `getUsers()` pide la información al servidor, pero esta es una ruta protegida de la API, por lo que si no estamos logueados con el token correcto o si el mismo ha expirado no podremos ver dicha información y seremos redirigidos.


# Componentes

## <code>**login.component.ts**</code>

```typescript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms'
import { Router } from '@angular/router';
import { UserLogin } from 'src/app/model/UserLogin';
import { LoginService } from 'src/app/services/login.service';


@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent implements OnInit {

  form:FormGroup

  constructor(private formBuilder: FormBuilder, private loginService: LoginService, private router:Router) {
    
    this.form = formBuilder.group(
      {
        username:[],
        password:[]
      }
    )

  }

  getData(){
    let username = this.form.get('username')?.value
    let password = this.form.get('password')?.value
    let userLogin = new UserLogin(username, password)

    this.loginService.login(userLogin).subscribe(res=>{
      console.log(res)
      sessionStorage.setItem('token', res.token)
      this.router.navigate(['/dashboard'])
    })
  }

  ngOnInit(): void {
  }
}
```
Si las credenciales son correctas, en este componente se recibe la respuesta del servidor con el token que se guardará en la sesión y se redirigirá al dashboard.

## <code>**dashboard.component.ts**</code>

```typescript
import { Component, OnInit } from '@angular/core';
import { UserDashboard } from 'src/app/model/Dashboard';
import { DashboardService } from 'src/app/services/dashboard.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: ['./dashboard.component.css']
})
export class DashboardComponent implements OnInit {

  userDashboard: UserDashboard = new UserDashboard()

  constructor(private dashboardService: DashboardService) {
  }

  ngOnInit(): void {
    this.getDataFromDashboard()
  }

getDataFromDashboard(){
  this.dashboardService.getUsers().subscribe(
    res => {
      console.log(res)
      this.userDashboard=res
        })
  }
}
```
En este componente se piden los datos del dashboard a la API, que solo los mostrará si son correctos.



# Vistas
Vistas HTML que reciben o envían datos.

## <code>**login.component.html**</code>

```html
<form [formGroup]="form" (submit)="getData()">
    <input type="text" name="username" formControlName="username" placeholder="usuario">
    <input type="text" name="password" formControlName="password" placeholder="contraseña">
    <button type="submit">Login</button>
</form>
```

## <code>**dashboard.component.html**</code>

```html
<p>{{userDashboard.username}}</p>

<p>{{userDashboard.email}}</p>
```


