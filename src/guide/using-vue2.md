# Adonis JS Project

## Instalación Adonis
AdonisJs es un framework MVC para Node. js que es compatible con la mayor parte de los sistemas operativos existentes, ofreciendo así un ecosistema estable sobre el que construir aplicaciones del lado del servidor. Además, AdonisJs incluye todas las herramientas necesarias para crear una API en tu servidor.

Instalamos con el siguiente comando en la consola.

``` sh
    npm i --global @adonisjs/cli
```

Comprobamos colocando adonis --versión

``` sh
   adonis --version
   4.0.13
```

## Creacion del proyecto

Para crear el proyecto colocamos el siguiente comando en la consola pero antes nos dirigimos a la direccion donde queremos que se creara el proyecto.

``` sh
    adonis new api-scp-project --api-only
```

y tendremos los siguientes comandos y los colocamos:

``` sh
    cd api-scp-project
    adonis serve --dev
```

## Dependencias del proyecto
Una vez pocicionados en el proyecto instalamos algunas dependencias para ellos necesitamos colocamos en la carpeta creada `api-scp-project` del proyecto, abrimos la consola y colocamos

``` sh
    npm install --save mysql
```

``` sh
    npm install --save pg
```

``` sh
    npm install --save url-parse
```

Con esto ya tenemos instalas todas las dependencias del proyecto.
## Configuración

Pasaremos a configurar el proyecto para ello primero abrimos el proyecto en un editor de texto y nos dirigimos a la siguiente ruta.

```
.
├─ api-scp-project
│  ├─ .env
│  └─ App
|    └─config  
│       └─ cors.js
|  └─start
|     └─kernel.js 

``` 

### .ENV

Desde la Línea 8 editamos con las credenciales de nuestra Base de Datos

 
``` .env
// Tipo de Conexión Base de datos
DB_CONNECTION=sqlite
// host
DB_HOST=127.0.0.1
//puerto
DB_PORT=3306
//usuario
DB_USER=root
//password
DB_PASSWORD=
// Nombre base de datos
DB_DATABASE=adonis
// Tipo de encriptación
HASH_DRIVER=bcrypt
``` 

Remplazamos por lo siguiente:

``` .env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_USER=root
DB_PASSWORD=
DB_DATABASE=api-adonis-scp
HASH_DRIVER=bcrypt
``` 
ahora creamos la base de datos con ese nombre en Xampp. abrimos la aplicacion xampp y habilitamos lo que es apache y mysql

<img :src="$withBase('/img/img1.png')" class="center">

en este mismo panel le damon en Admin en la parte de mysql lo cual nos abre el siguiente portal.

<img :src="$withBase('/img/img2.png')" class="center">

En la parte izquierda hay una opcion `Nueva` le damos click y nos enviar al siguiente portal en donde crearemos nuestra base de datos Colocando en el campo donde dice  `Nombre de la base de datos`
la configuracion que colocamos en el archibo `.ENV` que seria  `api-adonis-scp`

<img :src="$withBase('/img/img3.png')" class="center">

Le damos en click en Crear y ya tendriamos nuestra base de datos nueva lista para usarse.

<img :src="$withBase('/img/img4.png')" class="center">

### cors.js
Buscamos origin vemos que esta en estado False lo pasamos a true

``` js
'use strict'

module.exports = {
  
  origin: true,
    /**
     * 
     * 
     * 
    */
}
``` 

### kernel.js
Buscamos la variable const `serverMiddleware` y descomentamos   `//'Adonis/Middleware/Static',` quitándole las // iniciales

``` js
const serverMiddleware = [
  'Adonis/Middleware/Static',
  'Adonis/Middleware/Cors'
]
``` 

## Migraciones DB

Llamamos migración de datos al proceso que necesitamos hacer para transferir los datos de un sistema a otro mientras cambiamos el sistema de almacenamiento donde se encuentran los datos, o bien mientras se practican las modificaciones necesarias en la base de datos o la aplicación que los gestiona.

Creamos los modelos que son los encargados de hacer la conexión a las tablas y conjuntamente las migraciones con el siguiente comando en la consola.

Para tabla Categorías

``` sh
    adonis make:model Category -mc
```

Para tabla item-scp

``` sh
    adonis make:model ItemScp -mc
```

Nos dirigimos a la carpeta migraciones y encontraremos nuestras migraciones con un prefijo de números

```
.
├─ api-scp-project
│  └─ database
│     └─ migrations
|           └─.........

```

Abrimos en el editor el de categoría y agregamos el siguiente código

``` js
class CategorySchema extends Schema {
  up () {
    this.create('categories', (table) => {    
      table.increments()
      table.string('name')
      table.enu('danger', ['one', 'two', 'three']).defaultTo('one').nullable()
      table.timestamps()
    })
  }

  down () {
    this.drop('categories')
  }
}
``` 

Ahora lo mismo para para item_scp con el siguiente código

``` js
class ItemScpSchema extends Schema {
  up () {
    this.create('item_scps', (table) => {
      table.increments()
      table.string('name')
      table.string('item',100)
      table.text('descrition')
      table.string('avatar')

      table.integer('category_id').unsigned();
      table.foreign('category_id').references('categories.id');

      table.integer('user_id').unsigned();
      table.foreign('user_id').references('users.id');

      table.string("ip_creator",120).nullable();
      table.timestamps()
    })
  }

  down () {
    this.drop('item_scps')
  }
}
``` 
diagrama de base de datos

<img :src="$withBase('/img/diagrama.png')" class="center">

## Seeders
Los Seeders que sirven para inicializar las tablas con datos. Así como las migraciones nos permiten especificar el esquema de la base de datos, los seeders nos permiten también por medio de código alimentar las tablas con datos.

creamos nuestro seeder con el siguiente comando

``` sh
    adonis make:seed Database
```

ahora podemos apreciar que se creo una nueva carpeta dentro de la carpeta de `database` llamada `seeds` en el cual encontraremos un archibo llamado `DatabaseSeeder.js`   

```
.
├─ api-scp-project
│  └─ database
│     └─ migrations
|     ├─ seed
|         └─DatabaseSeeder.js    

```

en el archibo `DatabaseSeeder.js` colocaremos el siguiente codigo: 

``` js
const Category = use('App/Models/Category');

class DatabaseSeeder {
  async run () {
    await Category
      .createMany([
        {
          name:"Seguro",
          danger:"one"
        },
        {
          name:"Euclid",
          danger:"one"
        },
        {
          name:"Keter",
          danger:"one"
        },
        {
          name:"Neutralizado",
          danger:'two'
        },
        {
          name:"Explicado",
          danger:'two'
        },
        {
          name:"Thaumiel",
          danger:'three'
        },
        {
          name:"Apollyon",
          danger:'three'
        },
        {
          name:"Desmantelado",
          danger:'three'
        }
      ]);
  }
}
``` 

Terminada esta Acción ejecutamos el siguiente comando en consola 

``` sh
    adonis migration:refresh --seed
```

Devolviendonos lo siguiente 

``` sh
    Already at the last batch
    migrate: 1503250034279_user.js
    migrate: 1503250034280_token.js
    migrate: 1666908618030_category_schema.js
    migrate: 1666908721553_item_scp_schema.js
    Database migrated successfully in 1.15 s
    Seeded database in 591 ms
```
Ahora podemos observar el panel de la base de datos en XAMPP y veremos que se crearon las tablas y se llenaron los datos del seeder para Categoría.

<img :src="$withBase('/img/img5.png')" class="center">

## Controladores
Controlador que actúa como intermediario entre el Modelo y la Vista, gestionando el flujo de información entre ellos y las transformaciones para adaptar los datos a las necesidades de cada uno.

Tenemos creados 2 controladores respectivamente para Categoría y itemscp pero este proyecto necesita un controlador para la autenticación de usuarios por ello crearemos 1 más con el siguiente comando en consola

``` sh
    adonis make:controller Auth 
```
Escogemos `for http request`

y nos mostrara el siguiente mensaje
``` sh
   > Select controller type For HTTP requests
   √ create  app\Controllers\Http\AuthController.js
```

Vamos la dirección para encontrar el archivo `AuthController.js`

y agregamos le siguiente código

``` js
'use strict'

const User = use('App/Models/User');

class AuthController {
  async login ({request, response, auth}) {
    const  {user}  = request.all();
    const logged = await auth.attempt(user.email, user.password, true);
    return response.json(logged);
  }

  async register({request, response, auth }) {
    const userInstance = new User();
    const {user}  = request.all();

    userInstance.username = user.email;
    userInstance.email = user.email;
    userInstance.password = user.password;

    await userInstance.save();

    let token = await auth.generate(userInstance)
    Object.assign(userInstance, token)

    return response.json(userInstance);
  }
}

module.exports = AuthController
```

Ahora pasamos al siguiente controlador que sería  `CategoryController.js` y agregamos después de `'use strict'`
El siguiente código:

``` js
    
const Category = use('App/Models/Category');

```

### Category index 
Tenemos la función básica índex 

``` js
  async index ({ request, response, view }) {
  }
```

Agregamos el siguiente código

``` js
  async index ({ request, response, view }) {
    const categories =  await Category.all();
    return response.json(categories);
  }
```

### Category store 
Tenemos la función básica store 

``` js
  async store ({ request, response }) {
  }
```

Agregamos el siguiente código

``` js
 async store ({ request, response }) {
    const {categoria} = request.all();
    const NewCategoria = new Category();
    
    NewCategoria.name = categoria.name 
    NewCategoria.danger = categoria.danger

    await NewCategoria.save()
    return response.json(NewCategoria)
  }
```

### Category update 
Tenemos la función básica update 

``` js
 async update ({ params, request, response }) {
  }
```

Agregamos el siguiente código

``` js
  async update ({ params, request, response }) {
    const {id} = params;
    const UpDcategoria = await Category.find(id)
    const {categoria} = request.all();

    UpDcategoria.name = categoria.name;
    UpDcategoria.danger = categoria.danger;
    await UpDcategoria.save();
    return response.json(UpDcategoria);

  }
```

### Category destroy 
Tenemos la función básica destroy 

``` js
  async destroy ({ params, request, response }) {
  }
```

Agregamos el siguiente código

``` js
   async destroy ({ params, response }) {
    const {id} = params;
    const Delcategoria = await Category.find(id)
    await Delcategoria.delete();
    return response.json(Delcategoria);
  }
```

Ahora pasamos al siguiente controlador que sería  `ItemScpController.js` y agregamos después de `'use strict'`
El siguiente código:

``` js
    
const Helpers = use('Helpers');
const ItemScp = use('App/Models/ItemScp');

```
### ItemScp index

Tenemos la función básica índex 

``` js
  async index ({ request, response, view }) {
  }
```

Agregamos el siguiente código

``` js
  async index ({ request, response }) {
    const spcs = await ItemScp.all()
    return response.json(spcs)
  }
```

### ItemScp store

Tenemos la función básica store 

``` js
  async store ({ request, response }) {
  }
```

Agregamos el siguiente código

``` js
 async store ({ auth,request, response }) {
    const user = await auth.getUser();
    const scp = request.all();
    const newSpcs = new ItemScp();
    const avatar = request.file('avatar',{
        types:['image'],
        size:'2mb'
    })
    
    newSpcs.name        = scp.name
    newSpcs.item        = scp.item
    newSpcs.descrition = scp.descrition 
    newSpcs.category_id = scp.category_id
    newSpcs.user_id     = user.id
    newSpcs.avatar = new Date().getTime()+"."+avatar.subtype;
    await avatar.move(Helpers.publicPath('uploads'),{
      name: newSpcs.avatar
    })
    if(!avatar.moved()){
      return "ocurrio un error";
    }
    await newSpcs.save()
     
    
    return response.json(newSpcs)
  }
```

### ItemScp update

Tenemos la función básica update 

``` js
 async update ({ params, request, response }) {
  }
```

Agregamos el siguiente código

``` js
 async update ({ auth,params, request, response }) {
    const user = await auth.getUser();
    const scp = request.all();
    const {id} = params;
    const UdpSpcs = await ItemScp.find(id)
    const avatar = request.file('avatar',{
      types:['image'],
      size:'2mb'
    })
    
    UdpSpcs.name        = scp.name
    UdpSpcs.item        = scp.item
    UdpSpcs.descrition  = scp.descrition 
    UdpSpcs.category_id = scp.category_id
    UdpSpcs.user_id     = user.id
    UdpSpcs.avatar = new Date().getTime()+"."+avatar.subtype;
    await avatar.move(Helpers.publicPath('uploads'),{
      name: UdpSpcs.avatar
    })
    if(!avatar.moved()){
      return "ocurrio un error";
    }

    await UdpSpcs.save()

    return response.json(UdpSpcs)
  }
```

### ItemScp destroy
Tenemos la función básica destroy 

``` js
  async destroy ({ params, request, response }) {
  }
```

Agregamos el siguiente código

``` js
 async destroy ({ params, response }) {
    const {id} = params;
    const DeleteSpcs = await ItemScp.find(id);
    await DeleteSpcs.delete();
    return response.json(DeleteSpcs);
  }
```

## Rutas
En informática, una ruta (path, en inglés) es la forma de referenciar un archivo informático o directorio en un sistema de archivos de un sistema operativo determinado. Una ruta señala la localización exacta de un archivo o directorio mediante una cadena de caracteres concreta.

Ahora nos dirigimos a las rutas las encontramos en la siguiente carpeta del proyecto

```
.
├─ api-scp-project
│  └─ start
│     └─ routes.js

```

y colocamos el siguiente código debajo de todo.

``` js
Route.group(() => {
  Route.post('login', 'AuthController.login');
  Route.post('register', 'AuthController.register');
  Route.put('profile', 'AuthController.profile').middleware(['auth:jwt']);

  Route.get('categorias', 'CategoryController.index');
  Route.post('categorias', 'CategoryController.store').middleware(['auth:jwt']);
  Route.put('categorias/:id', 'CategoryController.update').middleware(['auth:jwt']);
  Route.delete('categorias/:id', 'CategoryController.destroy').middleware(['auth:jwt']);

  Route.get('scp-items', 'ItemScpController.index');
  Route.post('scp-items', 'ItemScpController.store').middleware(['auth:jwt']);
  Route.put('scp-items/:id', 'ItemScpController.update').middleware(['auth:jwt']);
  Route.delete('scp-items/:id', 'ItemScpController.destroy').middleware(['auth:jwt']);
}).prefix('api/v1');
``` 
Y con ello tenemos definidas nuestras rutas.

## Postman

Podemos probar las rutas en el postman:

::: tip
Ejercicio en clase
[Proyecto](https://drive.google.com/file/d/1Ghiihi5EPqQ_xsTCMpCwk-LM2NPUCDao/view)
:::

## Proyecto

Url del proyectos Back completo [proyect](https://github.com/waready/api-scp-project)

## Swagger

Swagger es un conjunto de herramientas de software de código abierto para diseñar, construir, documentar, y utilizar servicios web RESTful. Fue desarrollado por SmartBear Software e incluye documentación automatizada, generación de código, y generación de casos de prueba. [Doc](https://www.npmjs.com/package/adonis-swagger/v/1.2.0)

### Instalación

```sh
  npm install --save adonis-swagger
``` 

Register `SwaggerProvider` in `start/app.js`:

```js
    const providers = [
    ...
    'adonis-swagger/providers/SwaggerProvider'
  ]
``` 

```sh
  adonis swagger:export
``` 

### Configuración
modificamos la siguiente ruta `config/swagger.js`

```js
'use strict';

const swaggerDefinition = {
  openapi: '3.0.0',
  info: {
    title: 'Sample API',
    description: '',
    version: '1.0.0',
    contact: {
      email: 'name@gmail.com'
    }
  },
  
  components: {
    securitySchemes: {
      bearerAuth: {
        type: 'http',
        scheme: 'bearer',
        bearerFormat: 'JWT',
      }
    },  
  },
  security: [{
    bearerAuth: []
  }]
};

module.exports = {
  enable: true,
  specUrl: '/swagger.json',

  options: {
    swaggerDefinition: swaggerDefinition,
    // Incluir rutas de controladores en la documentación Swagger
    apis: [
      'app/Controllers/Http/AuthController.js',
      'app/Controllers/Http/CategoryController.js',
      'app/Controllers/Http/ItemScpController.js',
      'start/routes.js'
    ]
  }
};

``` 

### Documentación desde rutas 

```js
  'use strict'

  /*
  |--------------------------------------------------------------------------
  | Routes
  |--------------------------------------------------------------------------
  |
  | Http routes are entry points to your web application. You can create
  | routes for different URLs and bind Controller actions to them.
  |
  | A complete guide on routing is available here.
  | http://adonisjs.com/docs/4.1/routing
  |
  */

  /** @type {typeof import('@adonisjs/framework/src/Route/Manager')} */
  const Route = use('Route')

  Route.get('/', () => {
    return { greeting: 'Hello world in JSON' }
  })

  Route.group(() => {
    Route.post('login', 'AuthController.login');
    Route.post('register', 'AuthController.register');
    Route.put('profile', 'AuthController.profile').middleware(['auth:jwt']);

  /**
   * @swagger
   * /api/v1/categorias:
   *   get:
   *     tags:
   *       - Categories
   *     summary: Get categories
   *     description: Retrieve all categories
   *     security:
   *       - bearerAuth: []
   *     responses:
   *       200:
   *         description: Categories retrieved successfully
   *       401:
   *         description: Unauthorized
   */
  Route.get('categorias', 'CategoryController.index');

  /**
   * @swagger
   * /api/v1/categorias:
   *   post:
   *     tags:
   *       - Categories
   *     summary: Create category
   *     description: Create a new category
   *     security:
   *       - bearerAuth: []
   *     requestBody:
   *       required: true
   *       content:
   *         application/json:
   *           schema:
   *             $ref: '#/components/schemas/Category'
   *     responses:
   *       201:
   *         description: Category created successfully
   *       401:
   *         description: Unauthorized
   */
  Route.post('categorias', 'CategoryController.store').middleware(['auth:jwt']);

  /**
   * @swagger
   * /api/v1/categorias/{id}:
   *   put:
   *     tags:
   *       - Categories
   *     summary: Update category
   *     description: Update an existing category
   *     security:
   *       - bearerAuth: []
   *     parameters:
   *       - in: path
   *         name: id
   *         required: true
   *         description: ID of the category to update
   *         schema:
   *           type: integer
   *     requestBody:
   *       required: true
   *       content:
   *         application/json:
   *           schema:
   *             $ref: '#/components/schemas/Category'
   *     responses:
   *       200:
   *         description: Category updated successfully
   *       401:
   *         description: Unauthorized
   *       404:
   *         description: Category not found
   */
  Route.put('categorias/:id', 'CategoryController.update').middleware(['auth:jwt']);

  /**
   * @swagger
   * /api/v1/categorias/{id}:
   *   delete:
   *     tags:
   *       - Categories
   *     summary: Delete category
   *     description: Delete an existing category
   *     security:
   *       - bearerAuth: []
   *     parameters:
   *       - in: path
   *         name: id
   *         required: true
   *         description: ID of the category to delete
   *         schema:
   *           type: integer
   *     responses:
   *       204:
   *         description: Category deleted successfully
   *       401:
   *         description: Unauthorized
   *       404:
   *         description: Category not found
   */
  Route.delete('categorias/:id', 'CategoryController.destroy').middleware(['auth:jwt']);

  Route.get('scp-items', 'ItemScpController.index');
  Route.post('scp-items', 'ItemScpController.store').middleware(['auth:jwt']);
  Route.put('scp-items/:id', 'ItemScpController.update').middleware(['auth:jwt']);
  Route.delete('scp-items/:id', 'ItemScpController.destroy').middleware(['auth:jwt']);

  }).prefix('api/v1');

```

### Documentación desde controlador

```js
'use strict'

const User = use('App/Models/User');

class AuthController {
  /**
   * @swagger
   * /api/v1/login:
   *   post:
   *     tags:
   *       - Auth
   *     summary: Login
   *     description: User login
   *     requestBody:
   *       required: true
   *       content:
   *         application/json:
   *           schema:
   *             type: object
   *             properties:
   *               user:
   *                 type: object
   *                 properties:
   *                   email:
   *                     type: string
   *                   password:
   *                     type: string
   *     responses:
   *       200:
   *         description: Successful login
   *       401:
   *         description: Unauthorized
   */
  async login ({request, response, auth}) {
    const  {user}  = request.all();
    const logged = await auth.attempt(user.email, user.password, true);
    return response.json(logged);
  }

  /**
   * @swagger
   * /api/v1/register:
   *   post:
   *     tags:
   *       - Auth
   *     summary: Login
   *     description: User login
   *     requestBody:
   *       required: true
   *       content:
   *         application/json:
   *           schema:
   *             type: object
   *             properties:
   *               user:
   *                 type: object
   *                 properties:
   *                   email:
   *                     type: string
   *                   password:
   *                     type: string
   *     responses:
   *       200:
   *         description: Successful login
   *       401:
   *         description: Unauthorized
   */
  async register({request, response, auth }) {
    const userInstance = new User();
    const {user}  = request.all();

    userInstance.username = user.email;
    userInstance.email = user.email;
    userInstance.password = user.password;

    await userInstance.save();

    let token = await auth.generate(userInstance)
    Object.assign(userInstance, token)

    return response.json(userInstance);
  }

  /**
   * @swagger
   * /api/v1/profile:
    *   put:
   *     tags:
   *       - Auth
   *     summary: Update profile
   *     description: Update user profile
   *     security:
   *       - jwt: []
   *     parameters:
   *       - name: body
   *         in: body
   *         required: true
   *         schema:
   *           type: object
   *           properties:
   *             name:
   *               type: string
   *             email:
   *               type: string
   *     responses:
   *       200:
   *         description: Profile updated
   *       401:
   *         description: Unauthorized
   */
  async profile ({request, response, auth}) {
    let user = await auth.getUser();
    const userInput = request.input('user');
    user.email = userInput['email'];
    user.username = userInput['username'];
    await user.save();

    const logged = await auth.generate(user, true);
    return response.json(logged);
  }
}

module.exports = AuthController

```

### Ruta
accedemos a esta ruta:
```sh
  http://localhost:3333/docs/
```


## Publicacion servidor

Para subir a un servidor gratuito usaremos [Heroku](https://www.heroku.com/)
Es necesario que se registren y creen una cuenta

::: tip
Ejercicio en clase
:::


Para subir a un servidor de [Digital Ocean](https://cloud.digitalocean.com/)
Es necesario que se registren y creen una cuenta

### Instalar NVM (Node Version Manager)
```sh
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
  source ~/.bashrc  # o reinicia la terminal para cargar nvm
  nvm install node  # instala la última versión estable de Node.js
``` 

### Instalar MySQL
```sh
  sudo apt update
  sudo apt install mysql-server 
  sudo mysql_secure_installation
``` 

### Crear la base de datos
```sh
  mysql -u root -p  # accede a MySQL como root
  CREATE DATABASE api_adonis_scp;
``` 

### Clonar el repositorio:
```sh
  git clone https://github.com/waready/api-scp-project.git
  cd api-scp-project
```   

### Cambiar el archivo .env.example

```sh
  cp .env.example .env
  nano .env 
``` 

### Instalar dependencias

```sh
  npm install 
``` 

### Instalar PM2

```sh
  npm install -g pm2
``` 

### Iniciar la aplicación con PM2

```sh
  pm2 start server.js --name "api-scp-project"
``` 


