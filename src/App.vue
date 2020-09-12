<template>
  <div id="app">
   
  <div>
     <v-alert type="success" v-model="alert"  >
       <div>
         <v-btn small fab color="#5C6BC0" dark
          @click="alert = false">x</v-btn>
         Libro Eliminado
      </div>
     </v-alert>
  </div>

   <v-card
    color="grey lighten-4"
    flat
    height="40px"
    tile
  >
    <v-toolbar dense>
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

       <img alt="Vue logo" src="./assets/logo.png">

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-toolbar>
  </v-card>

   
     <v-form >
      <v-container>
       <v-row>

        <v-col cols="12" sm="6" md="3">
          <v-text-field label="Nombre" v-model="nombre"
           ></v-text-field>
        </v-col>

         <v-col cols="12" sm="6" md="3">
          <v-text-field label="Edicion" v-model="edicion" 
           ></v-text-field>
        </v-col>

        <v-col cols="12" sm="12" md="3">
          <v-btn small color="#2196F3" dark @click="addBook" >
            aceptar</v-btn>
        </v-col>        
      </v-row>
    </v-container>
  </v-form>
     <v-data-table
      :headers="headers"
      :items="listaLibros"
      :items-per-page="5"
      class="elevation-2"
  >
   <template v-slot:[`item.actions`]="{ item }">
 
      <v-btn small fab color="#FF1744" dark @click="deleteBook(item)" >
        <v-icon small>mdi-delete</v-icon>
      </v-btn> 
    
      <v-btn class="btn" small fab color="#8E24AA"
             dark @click="getBook(item)" >
        <v-icon small>mdi-pencil</v-icon>
      </v-btn> 
     
    </template>

  </v-data-table>
  </div>
</template>

<script>


export default {
   data() {
    return { 
       listaLibros:[],
       id:'',
       nombre:'',
       edicion:'',
       bandera:true,
       alert: false,
       url:"http://localhost/applibrolaravel/public/libro",
       headers: [
          {
            text: 'Libros',
            align: 'start',
            sortable: false,
            value: 'id',
          },
          { text: 'Nombre', value: 'nombre' },
          { text: 'Edicion', value: 'edicion' },
          { text: 'Actions', value: 'actions'  },
          
      ]
     }
  },
   methods:{
     addBook: async function(){
       let res 
       const obj = new FormData()
       obj.append("id",this.id)
       obj.append("nombre",this.nombre)
       obj.append("edicion",this.edicion)
       if(this.bandera){
          res = await this.$http.post(this.url, obj) 
          this.listaLibros.push(res.data.result)
       }else{
          res = await this.$http.post(this.url+"/update", obj)
          console.log(res.data) //sirve para ver respuesta del backend
          this.getBooks()
          this.id = ''
          this.bandera = true
       }
       this.nombre = ''
       this.edicion = '' 
     } ,

     getBooks: async function() { 
        const res = await this.$http.get(this.url)
        this.listaLibros = res.data      
     },

     deleteBook: async function({ id }){
        if(confirm('quieres eliminar')){
            const res = await this.$http.delete(this.url+`/${id}`)
            console.log(res.data);
            this.listaLibros=this.listaLibros
              .filter(book => book.id != res.data.id) 
            this.alert=true
         }
      },

      getBook:async function({ id }){
        const res = await this.$http.get(this.url+`/${id}`)
        console.log(res.data);
        this.id = res.data.id
        this.nombre = res.data.nombre
        this.edicion = res.data.edicion
        this.bandera = false    
      }    
    
   },
   created(){ 
     this.getBooks()
   },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}
img{ 
  width: 45px;
  height: 30px;
}
.btn{
  margin-left: 2px;
}

</style>
