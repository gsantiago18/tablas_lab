<template lang="pug">
  v-container.secundary(fill-height)
    v-row.justify-center.mt-2
            v-col(cols="2")
              v-btn(type="submit",color="success",@click="$i18n.locale='es'") ES
            v-col(cols="2")  
              v-btn(type="submit",color="primary",@click="$i18n.locale='en'") EN
    v-row
      v-col(cols="12")
        v-form(ref="myForm")
          h1.text-center {{ $t('message.inscription') }}
          v-text-field(
            v-model="newUser.name",
            v-bind:label="$t('form.name')"
            required
          )
          v-text-field(
            v-model="newUser.lastname",
            v-bind:label="$t('form.lastname')"
            required
          )
          v-text-field(
            v-model="newUser.id",
            v-bind:label="$t('form.id')"
            required
          )
          v-row.justify-center.mt-4
            v-col(cols="4")
              v-btn(type="submit",color="success",@click.prevent="addUser") {{ $t('message.adduser') }}
            v-col(cols="4")  
              v-btn(type="submit", color="disabled",@click.prevent="updateUser") {{ $t('message.updateus') }}
      v-col(cols="12")
        v-data-table(
          :headers="headers"
          :items="info"
        )      
          template(#item.opcion="{ item }")
            v-btn(color="primary" @click.prevent="handleEdit(item)") {{ $t('message.edit') }}
            v-btn(color="error" @click="handleDelete(item)") {{ $t('message.delete') }}
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
          { text: this.$t('form.name'), value: 'name' },
          { text: this.$t('form.lastname'), value: 'lastname' },
          { text: this.$t('form.id'), value: 'id' },
          { text: this.$t('form.option'), value: 'opcion' }
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
      async addUser() {
        if (this.newUser.name && this.newUser.lastname && this.newUser.id) {
          this.info.push({ ...this.newUser });
          this.newUser.name = "";
          this.newUser.lastname = "";
          this.newUser.id = null;
        }
        const lang=this.$i18n.locale
        try{
          const response = await this.$axios.post('/save',
           {locale: lang},
          {headers:{'Content-Type': 'application/json' }})
          alert(response.data.message)
        }
        catch(error){
          alert('Error al guardar:  ', error)
        }
      },
      async handleEdit(item){
        this.newUser.name=item.name
        this.newUser.lastname=item.lastname
        this.newUser.id=item.id

      },
      async updateUser() {
      const index = this.info.findIndex(user => user.id === this.newUser.id);
      if (index !== -1) {
        this.$set(this.info, index, { ...this.newUser });
        this.newUser = {
          name: '',
          lastname: '',
          id: ''
        }
        const lang=this.$i18n.locale
        try{
            const response = await this.$axios.post('/update',
            {locale: lang},
            {headers:{'Content-Type': 'application/json' }})
            alert(response.data.message)
          }
          catch(error){
            alert('Error al Actualizar:  ', error)
          }

      }
     },
      async handleDelete(item){
        const index = this.info.indexOf(item)
        if (index!==-1){
          this.info.splice(index,1)   
        }
        const lang=this.$i18n.locale
          try{
            const response = await this.$axios.delete('/delete',
            {locale: lang},
            {headers:{'Content-Type': 'application/json' }})
            alert(response.data.message)
          }
          catch(error){
            alert('Error al Eliminar:  ', error)
          }

      },
      setLocale(locale){
        this.$i18n.locale=locale
      },

      setup() {
      const t = inject('$t');
      return { t };
      }

    }
  }
  </script>

