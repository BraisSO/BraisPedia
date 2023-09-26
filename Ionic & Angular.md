# Ionic & Angular

### Comandos:
- `ionic start [name] [template] [options]`: Ejemplo -> *ionic start myApp tabs*.

- `ionic g page [nombre]`: Creación de todos los ficheros e inclusón del componente en el routing modules.
  
- `ionic serve`: Ejecuta el servidor y lanza la aplicación.


#### Apunte sobre colores
Algunos elementos de Ionic como los toolbar no permiten la asignación de background color desde el fichero scss, sino que se asigna como si fuese bootstrap *color=primary*, etc. Estos colores podemos verlos definidos en el archivo scss global que se encuentra en theme.

### Navegación
Usa la navegación típica de Angular con RouterLink desde HTML o en Typescript inicializando la clase *router* y usando sus metodos.

### Peticiones HTTP
Por defecto en los proyectos de Ionic no viene instalado *httpclient* por lo que tendremos que instalar las siguientes dependencias:

```
npm install cordova-plugin-advanced-http 
npm install @awesome-cordova-plugins/http 
ionic cap sync
```

Para usar la funcion *map* en las peticiones http se hace del siguiente modo:

**Servicio**
```typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable, map } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class TabsService {

constructor(private http:HttpClient) { }

getUsers(): Observable<string[]> {
  return this.http.get<any[]>("https://jsonplaceholder.typicode.com/users").pipe(
    map((res: any[]) => res.map(res => res.name)) 
    );
  
}
}
```

**Componente**
```typescript
export class Tab1Page {

  appUsers!:any[]

  constructor(private tabsService:TabsService) {

    this.tabsService.getUsers().subscribe((names: string[]) => {
      this.appUsers = names;
      
    })
  }
}

```

### Ionic Grid (ejemplo con cards)  ([ion-grid.](https://ionicframework.com/docs/api/grid))

```html
<ion-content>
  <ion-row>
    <ion-col sizeLg="6" sizeMd="6" sizeXs"12" *ngFor="let city of cities">
      <ion-card>
        <img src = "image">
        <div class = "card-title"> {{city.name}} </div>
      </ion-card>
    </ion-col>
  </ion-row>
<ion-content>
```

### Estilar botones en ionic-alert ([ion-alert.](https://ionicframework.com/docs/api/alert))

Cuando estemos trabajando con alertas con botones y queramos darles estilo, debemos hacerlo en una hoja de estilos global, ya que si intentamos hacerlo en la de los componentes no funcionará.

Por defecto nuestro proyecto tendrá el fichero *global.scss*.


### Barra de búsqueda ([ion-searchbar.](https://ionicframework.com/docs/api/searchbar))

**HTML**

```html
<ion-toolbar>
<ion-searchbar placeholder="Buscar" animated (ionChange)="searchCustomer($event)"></ion-searchbar>
</ion-toolbar>
```

**Typescript**

```typescript
searchCustomer(event){
  const text = event.target.value;
  this. searchedUser = this-users;
    if(text && text.trim() != ''){
      this.searchedUser = this.searchedUser.filter((user: any)=>{
        return (user.name.toLowerCase().indexOf(text.toLowerCase()) > -1);
    })
  }
}
    
```

### Recarga ([ion-refresher.](https://ionicframework.com/docs/api/refresher))

Está implementado de manera sencilla en la documentacion. Solo debemos acordarnos de hacer de nuevo la petición a la API dentro de la función que refresca y ya se nos recargaría la información 

### QR Scanner - capacitor ([QR - Scanner](https://github.com/capacitor-community/barcode-scanner))

**1. Instalación del componente:**
  ```
  npm install @capacitor-community/barcode-scanner
  npx cap sync
  ```

**2. Modificamos el `AndroidManifest.xml` añadiendo las líneas que contienen un `+`**
   
```xml
   <?xml version="1.0" encoding="utf-8"?>
<manifest
  xmlns:android="http://schemas.android.com/apk/res/android"
+ xmlns:tools="http://schemas.android.com/tools"
  package="com.example">

  <application
+    android:hardwareAccelerated="true"
  >
  </application>

+  <uses-permission android:name="android.permission.CAMERA" />

+  <uses-sdk tools:overrideLibrary="com.google.zxing.client.android" />
</manifest>

```

 **3. Creamos el HTML con el botón que ejecutará el escáner:**

```html
  <ion-button (click)="startScan()">Start Scan</ion-button>
```


**4. Funcionalidad básica en Typescript:**

```typescript
startScan = async () => {
  // Check camera permission
  // This is just a simple example, check out the better checks below
  await BarcodeScanner.checkPermission({ force: true });

  // make background of WebView transparent
  // note: if you are using ionic this might not be enough, check below
  BarcodeScanner.hideBackground();

  const result = await BarcodeScanner.startScan(); // start scanning and wait for a result

  // if the result has content
  if (result.hasContent) {
    console.log(result.content); // log the raw scanned content
  }
};
```

### Utiles

- Documentación Ionic: [Ionic Docs.](https://ionicframework.com/docs)
- Documentación Ionic: [Capacitor Docs.](https://capacitorjs.com/docs)
- Página de iconos de Ionic: [Ionic Icons](https://ionic.io/ionicons)
- [Obtener plataforma o sistema del dispositivo ](https://www.youtube.com/watch?v=EBxYajqQqT4&list=PLsngLoGbAagFEG-jwlpPhGsLzMSQ0tadP&index=24&pp=iAQB)
- [Obligar al usuario a actualizar la app ](https://www.youtube.com/watch?v=79eH7tpDj0M&list=PLsngLoGbAagFEG-jwlpPhGsLzMSQ0tadP&index=25&pp=iAQB)
- [Obtener la ubicación del dispotivo ](https://www.youtube.com/watch?v=KD15dCAUb-Q&list=PLsngLoGbAagFEG-jwlpPhGsLzMSQ0tadP&index=26&pp=iAQB)
  - [Plugin geolocalización necesario](https://ionicframework.com/docs/native/geolocation)
  - [Libreria para conversiones geográficas](https://github.com/Cap-go/capacitor-nativegeocoder#forwardgeocode)

- [Sqlite con Ionic](https://youtu.be/BM70fDqUo3c)
- [Sqlite con Ionic - Repo](https://github.com/jepiqueau/angular-sqlite-app-starter)
- [Debugging con Ionic](https://youtu.be/VN3VzMx8kSA)
- [Implementación de un *swipper*](https://youtu.be/01jwVb3WLoA)


