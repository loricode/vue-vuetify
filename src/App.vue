<template>
  <div id="app">
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
          <v-btn small color="primary" dark  @click="addBook" >
            aceptar</v-btn>
        </v-col>        
      </v-row>
    </v-container>
  </v-form>
     <v-data-table
      :headers="headers"
      :items="listaLibros"
      :items-per-page="5"
      class="elevation-1"
  ></v-data-table>
  </div>
</template>

<script>


export default {
   data() {
    return { 
       listaLibros:[],
       nombre:'',
       edicion:'',
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
      ]
     }
  },
   methods:{
     addBook: async function(){
       const obj = new FormData()
       obj.append("nombre",this.nombre)
       obj.append("edicion",this.edicion)
       const res = await this.$http.post(this.url, obj) 
       this.listaLibros.push(res.data.result)
       this.nombre = ''
       this.edicion = ''
     
     } ,
     getBooks: async function() { 
        const res = await this.$http.get(this.url)
        this.listaLibros = res.data      
     }, 
      
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

</style>
