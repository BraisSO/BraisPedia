# Crear proyecto React con Vite
Para crear un proyecto de React con la herramienta Vite, sigue estos pasos:

1. Abre una terminal y navega hasta el directorio en el que deseas crear tu proyecto.
2. Ejecuta el comando `npm create vite`.
3. Una vez creado el proyecto, entra en él con el comando `cd [folder]`.
4. Instala las dependencias necesarias con `npm install`.
5. Inicia el servidor de desarrollo con `npm run dev`.

# Segunda manera de crear un proyecto con React
Otra forma de crear un proyecto con React es utilizando el comando `create-react-app`:

1. Abre una terminal y ejecuta el comando `npx create-react-app ejemplo`.
2. Entra en el proyecto con `cd [folder]`.
3. Inicia el servidor con `npm start`.

# Levantar el servidor
Para levantar el servidor, debes ejecutar el script de ejecución especificado en el `package.json` de tu proyecto. Por ejemplo, `npm run [nombre del script de ejecución]`.

# Snippets componentes & otros
Para crear una estructura de componente en React, puedes utilizar las siglas `rfc`, `rfac`, etc.

La interpolación en React se realiza con `{}`.

Existen dos tipos de componentes en React: los componentes de clase y los componentes de función. Los componentes de clase utilizan la estructura de exportar una clase, mientras que los componentes de función exportan una función.

El DOM en React se renderiza dos veces. Para evitar esto, se puede usar `<React.StrictMode>`.

Para utilizar Bootstrap en tu proyecto React, debes instalarlo primero con `npm install bootstrap`. Luego, en tu archivo `App.jsx` debes importar el CSS y JS de Bootstrap:
<code>
// Bootstrap CSS
import "bootstrap/dist/css/bootstrap.min.css";
// Bootstrap Bundle JS
import "bootstrap/dist/js/bootstrap.bundle.min";
</code>

# Eventos
En React, las funciones deben ser declaradas dentro del componente y no llevan paréntesis.

Hooks es una característica de React que permite utilizar el estado y otras características de los componentes de clase en componentes de función. Para utilizar Hooks, debes importar `useState` en vez de `React`. Luego, puedes crear una constante como esta:

<code>const [counter, setCounter]=useState(0);</code>
