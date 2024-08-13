<template lang="pug">
  v-container.secundary(fill-height)
    v-row
      v-col(cols="12")
        v-form(ref="myForm")
          v-text-field(
            v-model="newUser.name",
            label="Nombre",
            required
          )
          v-text-field(
            v-model="newUser.lastname",
            label="Apellido",
            required
          )
          v-text-field(
            v-model="newUser.id",
            label="ID",
            required
          )
          v-btn(type="submit", color="success",@click.prevent="addUser") Agregar Usuario
          
          v-btn(type="submit", color="success",@click.prevent="updateUser") Actualizar Usuario
      v-col(cols="12")
        v-data-table(
          :headers="headers"
          :items="info"
        )      
          template(#item.opcion="{ item }")
            v-btn(color="primary" @click.prevent="handleEdit(item)") Editar
            v-btn(color="error" @click="handleDelete(item)") Eliminar
  </template>
  

  <script>
  export default {
    name: 'IndexPage',  
    data() {
      return {
        newUser: {
          name: "",
          lastname: "",
          id: null
        },
        headers: [
          { text: 'Nombre', value: 'name' },
          { text: 'Apellido', value: 'lastname' },
          { text: 'Cedula', value: 'id' },
          { text: 'Opcion', value: 'opcion' }
        ],
        info: [
          {
            name: 'Gabriel Santiago',
            lastname: 'Gonzalez',
            id: '401928'
          }
        ]
      };
    },
    methods: {
      addUser() {
        if (this.newUser.name && this.newUser.lastname && this.newUser.id) {
          this.info.push({ ...this.newUser });
          this.newUser.name = "";
          this.newUser.lastname = "";
          this.newUser.id = null;
        }
      },
      handleEdit(item){
        this.newUser.name=item.name
        this.newUser.lastname=item.lastname
        this.newUser.id=item.id

      },
      updateUser(item){
        const index = this.info.findIndex(user=>user.id==item.id)
        if(index!==-1){
          this.$set(this.info,index,{
            name:this.newUser.name,
            lastname:this.newUser.lastname,
            id:this.newUser.id
            
          })
          this.newUser = {
            name: '',
            lastname: '',
            id: ''
          };
        }

      },
      handleDelete(item){

      }

    }
  }
  </script>

