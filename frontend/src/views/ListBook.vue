<template>
  <div class="container">
    <div class="row">
      <div class="green lighten-3 text-center">
        <h2 >Listado de libros</h2>
      </div>
    </div>

    <div class="col-md-12">  
      <v-data-table dense :fields="fields" :items="books" item-key="name" class="elevation-1">
      </v-data-table>
    </div>
        
  </div>
</template>

<script>
export default {
  data() {
    return {
      fields:[
        { key: 'title', label: 'Titulo' },
        { key: 'description', label: 'Descripcion'},
        { key: 'action', label: 'Accion' }
      ],
      books: []
    }
  }
};
</script>
