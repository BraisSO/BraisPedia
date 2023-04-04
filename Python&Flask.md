# Python & Flask

#### Creacion de un entorno virtual de python

`python -m venv /path/to/new/virtual/environment`

#### Activar el entorno
`.\[path]\Scripts\Activate`

#### Dependencias a instalar dentro del entorno
`pip install flask flask-sqlalchemy flask-marshmallow marshmallow-sqlalchemy pymysql`

**sqlalchemy:** Esto importa la clase SQLAlchemy del paquete flask_sqlalchemy. SQLAlchemy es una biblioteca de Python para trabajar con bases de datos relacionales. Con Flask-SQLAlchemy, se puede integrar SQLAlchemy en una aplicación Flask de manera sencilla.

**marshmallow:** Esto importa la clase Marshmallow del paquete flask_marshmallow. Marshmallow es una biblioteca de serialización/deserialización de objetos para Python. Flask-Marshmallow es una extensión que proporciona integración con Marshmallow y Flask, lo que permite la serialización y deserialización de objetos en formato JSON.

**marshmallow-sqlalchemy:** es una extensión de Marshmallow que proporciona integración con SQLAlchemy. Se utiliza para definir esquemas de serialización/deserialización de objetos SQLAlchemy y proporciona un método sencillo para la definición de esquemas Marshmallow basados en modelos SQLAlchemy.

**pymysql:** es un controlador de base de datos MySQL para Python. Se utiliza para conectarse a una base de datos MySQL y ejecutar consultas SQL.


<hr>

##### Para que funcione la ejecución de la base de datos hay que agregar:

``` python
with app.app_context():

    db.create_all()

```


##### Recarga automática de los cambios hechos en la API => *(debug=true)*

```python
if __name__ == "__main__":
    app.run(debug=True) 
```

##### Si queremos cambiar el puerto, a este código debemos añadirle:

```python
if __name__ == "__main__":
    app.run(debug=True, port=8080) 
```

##### Funciones:

- Listar dependencias: <code> pip list </code>



<hr>


#### Ejemplo de controlador y CRUD todo en un mismo documento

```python
from flask import Flask,request,jsonify
from flask_sqlalchemy import SQLAlchemy
from flask_marshmallow import Marshmallow

app=Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI']='mysql+pymysql://root@localhost/flaskmysql'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS']=False

db=SQLAlchemy(app)
ma=Marshmallow(app)

#Creación del modelo que el URM transformará directamente en una tabla en la base de datos
class Task(db.Model):
    id=db.Column(db.Integer,primary_key=True)
    title=db.Column(db.String(70),unique=True)
    description=db.Column(db.String(100))
    
#Constructor 
    def __init__(self, title, description):
        self.title=title
        self.description=description
        


#Para ejecutar la base de datos tenemos que darle este contexto, sino no funciona.
with app.app_context():
    db.create_all()


#Define una clase "TaskSchema" que define cómo se serializarán y deserializarán las instancias de "Task" 
# (es decir, cómo se convertirán a y desde JSON). Esta clase Marshmallow tiene un atributo "Meta" que especifica 
# qué campos del modelo deben incluirse en la serialización. En este caso, "id", "title" y "description".
class TaskSchema(ma.Schema):
    class Meta:
        fields=('id','title','description')

task_schema=TaskSchema()

#Crea otra instancia de "TaskSchema" con el argumento many=True y la asigna a la variable 
# "tasks_schema". Esto indica que se espera una lista de objetos de la clase "Task" para ser serializados.
tasks_schema=TaskSchema(many=True)

#CRUD
#Método POST
@app.route('/tasks',methods=['POST'])
def create_task():
    
    title=request.json['title']
    description=request.json['description']
    
    new_task= Task(title,description)
    db.session.add(new_task)
    db.session.commit()
    
    return task_schema.jsonify(new_task)

#Método GET (todo)
@app.route('/tasks',methods=['GET'])
def get_tasks():
        all_tasks=Task.query.all()
        result=tasks_schema.dump(all_tasks)
        return jsonify(result)
    

#Método GET con parámetro
@app.route('/tasks/<id>', methods=['GET'])
def get_task(id):
    task = Task.query.get(id)
    return task_schema.jsonify(task)


#Método PUT
@app.route('/tasks/<id>', methods=['PUT'])
def updateTask(id):
    task = Task.query.get(id)
    
    title=request.json['title']
    description=request.json['description']
    
    task.title=title
    task.description=description
    
    db.session.commit()
    
    return task_schema.jsonify(task)

#Metodo DELETE
@app.route('/tasks/<id>', methods=['DELETE'])
def delete_task(id):
    task = Task.query.get(id)
    db.session.delete(task)
    db.session.commit()
    
    return task_schema.jsonify(task)

#Mensaje en ruta por defecto
@app.route('/', methods=['GET'])    
def index():
    return jsonify({'message': 'Welcome to my API'})


#Inicialización de la aplicación (algo así como el MAIN en JAVA)
if __name__ == "__main__":
    app.run(debug=True) 

```

Este código en Python utiliza el framework Flask para crear una API REST que se conecta a una base de datos MySQL y realiza operaciones CRUD (Crear, Leer, Actualizar y Eliminar) en una tabla llamada "Task". También utiliza las librerías SQLAlchemy y Marshmallow para manejar la interacción con la base de datos y la serialización y deserialización de objetos a JSON.

En resumen, lo que hace este código es:

- Importar las librerías Flask, SQLAlchemy y Marshmallow.
- Configurar la conexión a la base de datos MySQL y crear una instancia de la aplicación Flask.
- Definir un modelo llamado "Task" que representa una fila en la tabla de la base de datos.
- Crear una clase "TaskSchema" que define cómo se serializarán y deserializarán las instancias de "Task".
- Definir las rutas y los métodos HTTP (POST, GET, PUT, DELETE) para las operaciones CRUD en la tabla "Task".
- Inicializar la aplicación Flask y ejecutarla en modo de depuración.
- En la operación CREATE (POST), se recibe un objeto JSON que contiene los datos de una tarea ("title" y "description"), se crea una nueva instancia del modelo "Task" con estos datos y se guarda en la base de datos.

En la operación READ (GET), se obtienen todas las tareas de la tabla "Task" y se devuelven como una lista de objetos JSON.

En la operación READ (GET) con parámetro, se obtiene una tarea específica de la tabla "Task" mediante su identificador único (id) y se devuelve como un objeto JSON.

En la operación UPDATE (PUT), se recibe un objeto JSON que contiene los datos de una tarea actualizados ("title" y "description"), se obtiene la tarea correspondiente de la tabla "Task", se actualizan sus datos y se guardan los cambios en la base de datos.

En la operación DELETE, se elimina una tarea específica de la tabla "Task" mediante su identificador único (id) y se devuelve como un objeto JSON.

Por último, se define una ruta por defecto que devuelve un mensaje de bienvenida en formato JSON y se inicia la aplicación Flask.


**Nota**
Las funciones de <code>@app.route("url", "metodo")</code> se pueden abreviar llamando directamente al nombre del método. Es decir <code>@app.get("url")</code>, <code>@app.post("url")</code>, <code>@app.put("url")</code>, <code>@app.delete("url")</code>.

<hr>

#### Blueprints (importaciones)
Un "blueprint" en Python es una forma de organizar y modularizar una aplicación web Flask en secciones lógicas y separadas, lo que facilita la creación de aplicaciones web más grandes y complejas.

![BluePrint1](./Images/Python&Flask/blueprintController.PNG)
![BluePrint2](./Images/Python&Flask/blueprintMain.PNG)


El url_prefix es opcional, sirve para dar una ruta común a todos los endpoints de ese Blueprint.

<hr>

#### Arquetipo

**Estructura inicial**
![Arquetipo](./Images/Python&Flask/arquetipo.PNG)

**Dependencias / importaciones:**
<code>pip install 
flask 
flask-sqlalchemy 
flask-marshmallow 
marshmallow-sqlalchemy 
python-dotenv 
flask-cors 
mysqlclient *existe otro mysql-client pero no funciona en este caso*</code>


**Configuración de las variables de entorno, configuración de la base de datos y cadena de conexión**
Los archivos .env (también conocidos como archivos de configuración) son archivos de texto que se utilizan para almacenar variables de entorno. En un proyecto de Python-Flask, los archivos .env se utilizan para definir variables de entorno específicas para un entorno de implementación particular (por ejemplo, variables de configuración de bases de datos, claves de API, contraseñas, etc.). Los archivos .env son una forma segura y conveniente de administrar las variables de entorno, ya que no es necesario almacenarlas en el código fuente de la aplicación y se pueden mantener separadas del código.

Ficheros modificados:
- */dev/.env*
- */dev/dev_env.py*
- *main. py*

*En la cadena de conexión de la imagen falta se podría añadir el modulo pymysql*:

![dev_env_y_conexionDB](./Images/Python&Flask/dev_env_y_conexionDB.PNG)


**Configuración de la base de datos**
Aquí realizamos las configuraciones de la base de datos. Creación de las tablas, mapeo del objeto y sus características y realizamos un schema con *Marshmallow* que nos va a permitir serializar los objetos que recuperamos y transformarlos directamente en JSON.
![operacionesDB](./Images/Python&Flask/operacionesDB.PNG)

Esta es la estructura básica de una petición GET. El user repo hace la consulta a la base de datos a través de los métodos de SQLAlchemy. La información recuperada es pasada al controlador que a través de los *schemas* que defina serializa la información gracias a los métodos *Marshmallow* y por último el retorno de esta función es pasada a la ruta que nos las devuelve ya en formato JSON.

![estructuraPeticionGet](./Images/Python&Flask/estructuraPeticionGet.PNG)

Tras realizar estas configuraciones así es como quedaría el *main. py* de nuestra aplicación

![mainPy2](./Images/Python&Flask/mainPy2.PNG)

**get by ID & get by username:**
Lo más llamativo de esta parte es ir pasando el id de una función a otra y la función para buscar por username, que necesita ir recogiendo los datos en un diccionario a través de un bucle.

![Get by id & Get by username](./Images/Python&Flask/getbyid_getbyusername.PNG)


**Método Post**
Recuperamos cada uno de los valores que queremos guardar con el request 
![Post](./Images/Python&Flask/metodoPost.png)

**Método Put**
Similar al POST. Recuperamos el id del que queremos modificar, le asignamos unos nuevos valores y lo mandamos a la BD.
![Put](./Images/Python&Flask/update.PNG)

**Método Delete**
![Put](./Images/Python&Flask/delete.PNG)


**Habilitar CORS**
![CORS](./Images/Python&Flask/cors.PNG)


<hr>

#### Relaciones entre tablas



