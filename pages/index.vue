<template lang="pug">
  v-container.secundary(fill-height)
    v-row.justify-center.mt-2
            v-col(cols="2")
              v-btn(type="submit",color="success",@click="setlocale('es')") ES
            v-col(cols="2")  
              v-btn(type="submit",color="primary",@click="setlocale('en')") EN
    v-row
      v-col(cols="12")
        v-form(ref="myForm")
          h1.text-center INSCRIPCION DE USUARIOS
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
          v-row.justify-center.mt-4
            v-col(cols="4")
              v-btn(type="submit",color="success",@click.prevent="addUser") Agregar Usuario
            v-col(cols="4")  
              v-btn(type="submit", color="disabled",@click.prevent="updateUser") Actualizar Usuario
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

import { inject } from 'vue';

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
        ],
      };
    },
    methods: {
      addUser() {
        if (this.newUser.name && this.newUser.lastname && this.newUser.id) {
          this.info.push({ ...this.newUser });
          this.newUser.name = "";
          this.newUser.lastname = "";
          this.newUser.id = null;
          alert('Usuario agregado exitosamente')
        }
      },
      handleEdit(item){
        this.newUser.name=item.name
        this.newUser.lastname=item.lastname
        this.newUser.id=item.id

      },
      updateUser() {
      const index = this.info.findIndex(user => user.id === this.newUser.id);
      if (index !== -1) {
        this.$set(this.info, index, { ...this.newUser });
        this.newUser = {
          name: '',
          lastname: '',
          id: ''
        };
      }
     },
      handleDelete(item){
        const index = this.info.indexOf(item)
        if (index!==-1){
          this.info.splice(index,1)
          alert('Usuario eliminado exitosamente')
        }

      },
      setlocale(newlocale){
        this.$i18n.locale=newlocale
      },
      setup() {
      const t = inject('$t');
      return { t };
      }

    }
  }
  </script>

