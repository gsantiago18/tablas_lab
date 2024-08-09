<template lang="pug">
v-container.secundary(fill-height)

  h1.text-h1.green--text hola mundo
    span.blue--text Hola mundo
  h2 hola mundo
  h3 hola mundo
  v-alert(type="success") Proceso Exitoso


  v-alert(type="error") Cuidado! Ocurrio un error

  v-data-table(:headers="headers" :items="info")
    template(#item.opcion="{ item }")
      
      v-btn(color="primary" @click="handleEdit(item)") Editar
      v-btn(color="error" @click="handleDelete(item)") Eliminar
      

  

</template>

<script>
export default {
  name: 'IndexPage',
  data(){
    return{
      headers:[{
        text:'Nombre',
        value:'name'
      },
      {
        text:'Apellido',
        value:'lastname'
      },
      {
        text:'Apellido',
        value:'lastname'
      },{
        text:'Cedula',
        value:'id'
      },
      {
        text:'Opcion', 
        value:'opcion' 
      }
      ],
      info:[
        {
        name:'Gabriel Santiago',
        lastname:'Gonzalez',
        id:'401928'
      },
      {
        name:'Gabriel Santiago',
        lastname:'Gonzalez',
        id:'401928'
      },
      {
        name:'Gabriel Santiago',
        lastname:'Gonzalez',
        id:'401928'
      },
      {
        name:'Gabriel Santiago',
        lastname:'Gonzalez',
        id:'401928'
      },
      ]
    }
  }
}
</script>
