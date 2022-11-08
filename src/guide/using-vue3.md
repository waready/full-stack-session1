# Vue JS Project

## Instalación Vuejs
Vue.js es un framework de JavaScript de código abierto para la construcción de interfaces de usuario y aplicaciones de una sola página.

Instalamos con el siguiente comando en la consola.

``` sh
    npm install -g @vue/cli
    # OR
    yarn global add @vue/cli
```

Comprobamos colocando adonis --versión

``` sh
   vue --version
```

## Creacion del proyecto

Para crear el proyecto colocamos el siguiente comando en la consola pero antes nos dirigimos a la direccion donde queremos que se creara el proyecto.

``` sh
    vue create vue-scp-proyect
```

y tendremos los siguientes comandos y los colocamos:

``` sh
    Manually select features
    (*) Choose Vue version
    (*) Babel
    ( ) TypeScript
    ( ) Progressive Web App (PWA) Support        
    (*) Router
    (*) Vuex
    ( ) CSS Pre-processors
    (*) Linter / Formatter
    ( ) Unit Testing
    >( ) E2E Testing

    > 2.x
    3.x

    cd vue-scp-proyect
    yarn serve
```

## Configuración

Pasaremos a configurar el proyecto para ello primero abrimos el proyecto en un editor de texto y nos dirigimos a la siguiente ruta.

```
.
├─ vue-scp-proyect
│  └─ Src
|     └─components
│         └─ HelloWorld.vue

```
Eliminamos el archibo  `HelloWorld.vue` 

## Dependencias del proyecto
Una vez pocicionados en el proyecto instalamos algunas dependencias para ellos necesitamos colocamos en la carpeta creada `vue-scp-proyect` del proyecto, abrimos la consola y colocamos

Vuetify

``` sh
    vue add vuetify
    Vuetify 2 - Configure Vue CLI (advanced) 
    ? Choose a preset: Vuetify 2 - Configure Vue CLI (advanced)
    ? Use a pre-made template? (will replace App.vue and HelloWorld.vue) Yes
    ? Use custom theme? Yes
    ? Use custom properties (CSS variables)? Yes
    ? Select icon font Material Icons
    ? Use fonts as a dependency (for Electron or offline)? No
    ? Use a-la-carte components? No
    ? Use babel/polyfill? No        
    ? Select locale English
```
Axios 

``` sh
    npm i axios --s
```
Con esto ya tenemos instalas todas las dependencias del proyecto.


## Configuraciones

### Main.js

Nos dirigimos a la direccion en donde se encuentra el archibo `main.js` 

```
.
├─ vue-scp-project
│  └─ src
│     └─ main.js
|          

```

Abrimos en el editor y agregamos el siguiente código

``` js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import vuetify from './plugins/vuetify'
import axios from "axios";

// axios.defaults.baseURL="http://127.0.0.1:3333/api/v1/"
axios.defaults.baseURL="https://project-scp-adonis.herokuapp.com/api/v1/"
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'
let token = window.localStorage.getItem('_tokenOne')
if(token){
  axios.defaults.headers.post['Authorization'] = `Bearer ${window.localStorage.getItem('_tokenOne')}`
  axios.defaults.headers.put['Authorization'] = `Bearer ${window.localStorage.getItem('_tokenOne')}`
  axios.defaults.headers.delete['Authorization'] = `Bearer ${window.localStorage.getItem('_tokenOne')}`
    // "Access-Control-Allow-Origin": "*",
    //'Authorization': `Bearer ${window.localStorage.getItem('_tokenOne')}` 
}
else{
    router.push("/login")
}

Vue.config.productionTip = false

new Vue({
  router,
  store,
  vuetify,
  render: h => h(App)
}).$mount('#app')


``` 

### Store.js

Nos dirigimos a la direccion en donde se encuentra el archibo `index.js` 

```
.
├─ vue-scp-project
│  └─ src
│     └─ store
|         └─index.js   

```

Abrimos en el editor y agregamos el siguiente código


``` js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    user: null,
  },
  getters: {
  },
  mutations: {
    SetUserDetails(state,payload){
      console.log(payload)
      state.user = payload.email
    },
    salir(state){
      state.user = null
    }
  },
  actions: {
    loginUser({commit},payload){
      commit('SetUserDetails',{
        email:payload
      })
    },
    logout({commit}){
      commit('salir')
    }

  },
  modules: {
  }
})
``` 

### App.vue

::: tip
Colocamos el siguiente comando para verificar
:::

``` sh
    npm install
```

Nos dirigimos a la direccion en donde se encuentra el archibo `App.vue` 

```
.
├─ vue-scp-project
│  └─ src
│     └─ App.vue
|          

```

Abrimos en el editor y agregamos el siguiente código


``` vue
<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
        <h1 class="ml-2"> v.1.1.1</h1> 
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>open_in_new</v-icon>
      </v-btn>
      <v-menu offset-y class="mr-2" v-if="$store.state.user">
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="primary"
            dark
            v-bind="attrs"
            v-on="on"
          >
            {{ $store.state.user }}
          </v-btn>
        </template>
        <v-list>
          <v-list-item>
            <v-list-item-title @click="salir()">Salir</v-list-item-title>
          </v-list-item>
        </v-list>
    </v-menu>

      <v-btn v-else 
        @click="login()"
      >
      <span class="mr-2">Login</span>
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>
import { mapActions } from 'vuex';
import router from './router';
export default {
  name: 'App',

  data: () => ({
    //
  }),
  methods:{
    ...mapActions(['logout']),
    login(){
      let token = window.localStorage.getItem('_tokenOne')
        if(!token){
          router.push("/login")
        }
    },
    salir(){
      this.logout();
      localStorage.clear();
      sessionStorage.clear();
      router.push("/login")
    }
  }
};
</script>

``` 

## Components

nos dirigimos a la siguiente direccion  

```
.
├─ vue-scp-project
│  └─ src
│     └─ Components  
|          └─...........
```

### HelloWorld.vue

en el archibo `HelloWorld.vue` colocaremos el siguiente codigo: 


``` vue
<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img
          :src="require('../assets/logo.svg')"
          class="my-3"
          contain
          height="200"
        />
      </v-col>

      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to SCP - porjects 
        </h1>

        <p class="subheading font-weight-regular">
          Esta pequeña aplicacion trata de reunir todos los scp conocidos
          <br>para tener mas informacion de los scp puede visitar
          <a
            href="http://scp-es.com/"
            target="_blank"
          >La fundación SCP</a>
        </p>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <v-row justify="center">
          <v-btn
          class="ma-2"
          outlined
          color="indigo"
          to="/allscp"
          >
          Get STARTED
          </v-btn>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'HelloWorld',

    data: () => ({
    }),
  }
</script>

``` 


### Login.vue

Creamos el archibo `Login.vue` y colocaremos el siguiente codigo: 


``` vue

<template>
    <v-app id="inspire">
       <v-main>
          <v-container fluid fill-height>
             <v-layout align-center justify-center>
                <v-flex xs12 sm8 md4>
                   <v-card class="elevation-12">
                      <v-toolbar dark color="primary">
                         <v-toolbar-title>Login form</v-toolbar-title>
                      </v-toolbar>
                      <v-card-text>
                         <v-form>
                            <v-text-field
                               prepend-icon="person"
                               name="login"
                               label="Login"
                               v-model="email"
                               type="text"
                            ></v-text-field>
                            <v-text-field
                               id="password"
                               prepend-icon="lock"
                               name="password"
                               label="Password"
                               v-model="password"
                               type="password"
                            ></v-text-field>
                         </v-form>
                      </v-card-text>
                      <v-card-actions>
                         <v-spacer></v-spacer>
                         <v-btn @click="enviarAuth()"  color="primary" >Login</v-btn>
                      </v-card-actions>
                   </v-card>
                </v-flex>
             </v-layout>
          </v-container>
       </v-main>
    </v-app>
 </template>
 
 <script>
import axios from "axios";
import router from '@/router'
import { mapActions } from "vuex";
export default {
    name: 'Login',
    props: {
      source: String,
   },
   data() {
      return {
         email:"",
         password:""         
      }
   },
   methods:{
      ...mapActions(['loginUser']),
      enviarAuth(){
         const user = {
            email: this.email,
            password:this.password
         }
         axios.post('login', {user})
         .then( (response) => {
            window.localStorage.setItem('_tokenOne', response.data.token);
            this.loginUser(user.email)   
            router.push('/allscp');
         })
         .catch(function (error) {
            console.log(error);
         });
      }
   }
 };
 </script>
 
 <style></style>
 

``` 

### FormularioCategoria.vue

Creamos el archibo `FormularioCategoria.vue` y colocaremos el siguiente codigo: 


``` vue
<template>
    <v-app id="inspire">
       <v-main>
          <v-container fluid fill-height>
             <v-layout align-center justify-center>
                <v-flex xs12 sm8 md4>
                   <v-card class="elevation-12">
                      <v-toolbar dark color="primary">
                         <v-toolbar-title>Categoria form</v-toolbar-title>
                      </v-toolbar>
                      <v-card-text>
                         <v-form>
                            <v-text-field
                               prepend-icon="categoria"
                               name="categoria"
                               label="categoria"
                               v-model="name"
                               type="text"
                            ></v-text-field>
                            <v-select
                            v-model="danger"
                            :items="['one','two','three']"
                        
                            label="Peligro"
                            required
                            ></v-select>
                            
                         </v-form>
                      </v-card-text>
                      <v-card-actions>
                         <v-spacer></v-spacer>
                         <v-btn @click="enviarAuth()"  color="primary" >Login</v-btn>
                      </v-card-actions>
                   </v-card>
                </v-flex>
             </v-layout>
          </v-container>
       </v-main>
    </v-app>
 </template>
 
 <script>
import axios from "axios";
 export default {
    name: 'categoria',
   data() {
      return {
        name:"",
        danger:""         
      }
   },
   methods:{
      enviarAuth(){
         const categoria = {
            name: this.name,
            danger:this.danger
         }
         axios.post('categorias', {categoria},
        {
            headers: {
                "Access-Control-Allow-Origin": "*",
                'Authorization': `Bearer ${window.localStorage.getItem('_tokenOne')}` 
            } 
        })
         .then(function (response) {
            //window.localStorage.setItem('_tokenOne', response.data.token);
            console.log(response);
         })
         .catch(function (error) {
            console.log(error);
         });
      }
   }
 };
 </script>
 
 <style></style>
 
``` 

### FormularioSpc.vue

Creamos el archibo `FormularioSpc.vue` y colocaremos el siguiente codigo: 


``` vue
<template>
    <v-app>
      <v-container> 
      <v-row>
         <v-col>
            <v-card>
                <v-tabs
                background-color="deep-purple accent-4"
                center-active
                dark
                >
                <v-tab to="/allscp">Listado</v-tab>
                <v-tab  active>Crear</v-tab>
                </v-tabs>
            </v-card>
         </v-col>
      </v-row>
   </v-container>       
       <v-main>
          <v-container fluid fill-height>
   
             <v-layout align-center justify-center>
                <v-flex xs12 sm8 md4>
                   <v-card class="elevation-12">
                      <v-toolbar dark color="primary">
                         <v-toolbar-title>SCP Register</v-toolbar-title>
                      </v-toolbar>
                      <v-card-text>
                         <v-form>
                            <v-file-input
                            label="File input"
                            outlined
                            dense 
                            v-model="FILE" 
                            ref="elFile"
                            >
                            </v-file-input>
                            <v-text-field
                               name="scp-name"
                               label="scp-name"
                               v-model="name"
                               type="text"
                            ></v-text-field>
                            <v-text-field
                               name="scp-item"
                               label="scp-item"
                               v-model="item"
                               type="text"
                            ></v-text-field>
                            <v-textarea
                              name="input-7-1"
                              label="Descripción"
                              auto-grow
                              v-model="descrition"
                           ></v-textarea>
                            <v-select
                            v-model="category_id"
                            :items="mensaje"
                            label="Categoria"
                            required
                            ></v-select>
                            
                         </v-form>
                      </v-card-text>
                      <v-card-actions>
                         <v-spacer></v-spacer>
                         <v-btn @click="enviarAuth()"  color="primary" >Registrar Scp</v-btn>
                      </v-card-actions>
                   </v-card>
                </v-flex>
             </v-layout>
          </v-container>
       </v-main>
    </v-app>
 </template>
 
 <script>
import axios from "axios";
import router from "@/router";
 export default {
    name: 'scp',
 
   data() {
      return {
        FILE: null,
        name:"",
        item:"",
        descrition:"",
        category_id:"",
        mensaje:[]
      }
   },
   mounted(){
    axios.get('categorias')
    .then((respuesta) => { 
        console.log(respuesta);
        this.mensaje = respuesta.data.map(option => ({value: option.id, text: option.name}));
    });
    },
   methods:{
      enviarAuth(){
        
         const formData = new FormData();
            formData.append('avatar', this.FILE)
            formData.append("name", this.name)
            formData.append("item", this.item)
            formData.append("descrition", this.descrition)
            formData.append("category_id", this.category_id)
         axios.post('scp-items', formData,
        {
            headers: {
                "Access-Control-Allow-Origin": "*",
                'Content-Type': 'multipart/form-data',
                //'Authorization': `Bearer ${window.localStorage.getItem('_tokenOne')}` 
            } 
        })
         .then(function (response) {
            //window.localStorage.setItem('_tokenOne', response.data.token);
            console.log(response);
            router.push("/allscp")
         })
         .catch(function (error) {
            console.log(error);
         });
      }
   }
 };
 </script>
 
 <style></style>
 
``` 

### ListScp.vue

Creamos el archibo `ListScp.vue` y colocaremos el siguiente codigo: 


``` vue
<template>
  <div>
    <v-container>
      <v-card>
        <v-tabs background-color="deep-purple accent-4" center-active dark>
          <v-tab active>Listado</v-tab>
          <v-tab @click="CrearScp()">Crear</v-tab>
        </v-tabs>
      </v-card>
      <v-row>
        <v-card
          class="mx-auto my-12"
          max-width="374"
          v-for="item in mensaje"
          :key="item.id"
        >
          <template slot="progress">
            <v-progress-linear
              color="deep-purple"
              height="10"
              indeterminate
            ></v-progress-linear>
          </template>

          <v-img
            height="250"
            :src="
              'https://project-scp-adonis.herokuapp.com/uploads/' + item.url_img
            "
          ></v-img>

          <v-card-title>{{ item.name }}</v-card-title>

          <v-card-text>
            <div class="text-subtitle-1">$ • {{ item.item }}</div>

            <div>{{ item.descrition }}</div>
          </v-card-text>

          <v-divider class="mx-4"></v-divider>
          <v-card-text>
            <v-chip-group
              active-class="deep-purple accent-4 white--text"
              column
            >
              <v-chip>{{ item.category_id }}</v-chip>
            </v-chip-group>
          </v-card-text>
          <v-card-actions>
            <v-btn color="deep-purple lighten-2" text @click="ShowScp(item)">
              Editar
            </v-btn>

            <v-btn color="red lighten-2" text @click="EliminarScp(item.id)">
              Eliminar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-row>
    </v-container>

    <v-dialog v-model="dialog">
      <v-card>
        <v-card-title class="text-h5 grey lighten-2"> Editar SCP </v-card-title>

        <v-card-text class="mt-2">
          <v-form>
            <!--input type="file" @change="uploadFile()" ref="elFile"/-->
            <v-file-input
              label="File input"
              outlined
              dense
              v-model="FILE"
              ref="elFile"
            >
            </v-file-input>
            <v-text-field
              name="scp-name"
              label="scp-name"
              v-model="name"
              type="text"
            ></v-text-field>
            <v-text-field
              name="scp-item"
              label="scp-item"
              v-model="item"
              type="text"
            ></v-text-field>
            <v-textarea
              name="input-7-1"
              label="Descripción"
              auto-grow
              v-model="descrition"
            ></v-textarea>
            <v-select
              v-model="category_id"
              :items="categorias"
              label="Categoria"
              required
            ></v-select>
          </v-form>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn @click="EditarScp()" color="primary">Editar Scp</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
import axios from "axios";
import router from "@/router";
export default {
  name: "allscp",

  data() {
    return {
      mensaje: [],
      categorias: [],
      dialog: false,

      id: "",
      FILE: null,
      name: "",
      item: "",
      descrition: "",
      category_id: "",
    };
  },
  mounted() {
    this.ListarScp();
    axios.get("categorias").then((respuesta) => {
      console.log(respuesta);
      this.categorias = respuesta.data.map((option) => ({
        value: option.id,
        text: option.name,
      }));
    });
  },
  methods: {
    ListarScp() {
      axios.get("scp-items").then((respuesta) => {
        console.log(respuesta);
        this.mensaje = respuesta.data;
      });
      console.log("llego");
    },
    CrearScp() {
      let token = window.localStorage.getItem("_tokenOne");
      if (token) {
        router.push("/scp");
      } else {
        router.push("/login");
      }
    },
    ShowScp(item) {
      console.log(item);
      this.id = item.id;
      this.dialog = true;
      this.name = item.name;
      this.item = item.item;
      this.descrition = item.descrition;
      this.category_id = item.category_id;
    },
    EditarScp() {
      const formData = new FormData();
      formData.append("avatar", this.FILE);
      formData.append("name", this.name);
      formData.append("item", this.item);
      formData.append("descrition", this.descrition);
      formData.append("category_id", this.category_id);
      axios
        .put("scp-items/" + this.id, formData, {
          headers: {
            "Access-Control-Allow-Origin": "*",
            "Content-Type": "multipart/form-data",
            //'Authorization': `Bearer ${window.localStorage.getItem('_tokenOne')}`
          },
        })
        .then((response) => {
          console.log(response);
          this.ListarScp();
          this.dialog = false;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    EliminarScp(id) {
      this.ListarScp();
      if (confirm("¿Esta seguro de Eliminar este Scp?") == true) {
        axios
          .delete("scp-items/" + id, {
            // headers: {
            //     "Access-Control-Allow-Origin": "*",
            //     'Authorization': `Bearer ${window.localStorage.getItem('_tokenOne')}`
            // }
          })
          .then(() => this.ListarScp())
          .catch(function (error) {
            console.log(error);
          });
      } else {
        return;
      }
    },
  },
};
</script>
<style></style>

``` 

## Router

Nos dirigimos a la direccion en donde se encuentra el archibo `index.js` 

```
.
├─ vue-scp-project
│  └─ src
│     └─ router
|         └─index.js   

```

Abrimos en el editor y agregamos el siguiente código


``` js
import Vue from 'vue'
import VueRouter from 'vue-router'
import HomeView from '../views/HomeView.vue'
import Login from '@/components/Login.vue'
import FormulariCategoria from '@/components/FormularioCategoria.vue'
import FormulariScp from '@/components/FormularioSpc.vue'
import ListScp from '@/components/ListScp.vue'
Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/login',
    name: 'login',
    component: Login
  },
  {
    path: '/categoria',
    name: 'categoria',
    component: FormulariCategoria
  },
  {
    path: '/allscp',
    name: 'allscp',
    component: ListScp
  },
  {
    path: '/scp',
    name: 'scp',
    component: FormulariScp
  },
  // {
  //   path: '*',
  //   name: 'about',
  //   // route level code-splitting
  //   // this generates a separate chunk (about.[hash].js) for this route
  //   // which is lazy-loaded when the route is visited.
  //   component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  // },
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})


export default router

``` 


## Publicacion servidor

Para subir a un servidor gratuito usaremos [github](https://github.com/)
Es necesario que se registren y creen una cuenta

::: tip
Ejercicio en clase
:::


## Proyecto

Url del proyectos front completo [proyect](https://github.com/waready/vue-scp-proyect)