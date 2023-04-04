# JWT - Flask

JWT son las siglas de JSON Web Tokens, un estándar abierto que define un formato compacto y seguro para transmitir información entre dos partes como un objeto JSON.

Los JWT se utilizan comúnmente para la autenticación y la autorización en aplicaciones web y móviles, ya que proporcionan una forma segura y fácil de transmitir información de usuario entre el servidor y el cliente sin la necesidad de almacenar esa información en una base de datos.

**Paso 1: Instala las dependencias**
Para usar JWT con Python y Flask, necesitarás instalar las siguientes dependencias:

```
flask
flask_jwt_extended
```
Puedes instalarlas usando pip:
```
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

```
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