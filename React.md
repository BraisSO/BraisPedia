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

# Peticiones HTTP

En React, existen varias librerías populares para realizar peticiones HTTP, como Axios, Fetch, o la librería nativa de JavaScript, XMLHttpRequest. A continuación se presentan algunos ejemplos de cómo se pueden realizar peticiones HTTP en React utilizando Axios.

    Instalando Axios:

Antes de comenzar a realizar peticiones HTTP, es necesario instalar la librería Axios. Se puede hacer esto ejecutando el siguiente comando en la terminal:


    *npm install axios*

**Realizando una petición GET:**


    import { useState, useEffect } from 'react';
    import reactLogo from './assets/react.svg';
    import './App. CSS;
    import axios from 'axios';
    
    function App() {
    const [users, setusers] = useState([])

    useEffect( ( ) => {
    axios get ("https://jsonplaceholder.typicode.com/users")
         .then( res=> setUsers(res. data))
         .catch(error => console.log(error))
         },[])

         return

   *Imprimir datos petición:*

     <div>
     {
     users.map((user,i) =>{
       return(
         <p key={i}>{user.name} - {user.email}</p>
        )
      })
     }
     </div>
                                         I

 *Se añade [] al "useEffect" para terminar el bucle infinito de peticiones. [] -> es un array de dependencias vacío.*

 En este ejemplo, se importa la librería Axios y se utiliza el método `get()` para realizar una petición GET a la URL especificada. El resultado de la petición se almacena en el estado del componente y se utiliza para renderizar los datos en la vista.


**Realizando una petición POST:**
    
    import axios from 'axios';
    class ExampleComponent extends React.Component {
    state = {
    title: '',
    body: ''
    }

    handleSubmit = (e) => {
        e.preventDefault();
        const post = {
            title: this.state.title,
            body: this.state.body
        }
        axios.post('https://jsonplaceholder.typicode.com/posts', post)
        .then(res => {
            console.log(res.data);
        });
    }

    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <input 
                    type="text" 
                    name="title" 
                    value={this.state.title}
                    onChange={e => this.setState({ title: e.target.value })} 
                />
                <textarea 
                    name="body"
                    value={this.state.body}
                    onChange={e => this.setState({ body: e.target.value })}>
                </textarea>
                <button type="submit">Submit</button>
                </form>
                );
            }
        }

        export default ExampleComponent;

En este ejemplo, se utiliza el método `post()` para realizar una petición POST a la URL especificada. Se crea un objeto `post` con los datos a enviar y se pasa como segundo argumento al método `post()`. El resultado de la petición se puede manejar en la promesa `then()` para realizar cualquier acción necesaria, como mostrar un mensaje de éxito al usuario.