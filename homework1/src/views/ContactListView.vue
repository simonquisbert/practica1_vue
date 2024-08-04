<template>
    <div>
      <h1>{{ message }}</h1>
      <div>
        <div class="search-container">
          <input type="search" class="search-box" placeholder="Buscar..." @input="updateSearchQuery">
          <button class="search-button" @click="buscarContacto">Buscar</button>
        </div>
      </div>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Email</th>
            <th>Address</th>
            <th>Phone</th>
            <th>Country</th>
            <th>City</th>
            <th>Actions</th>
          </tr>
          <tr>
            <th></th>
            <th><input type="text" v-model="contactoNuevoObj.nombre"></th>
            <th><input type="text" v-model="contactoNuevoObj.email"></th>
            <th><input type="text" v-model="contactoNuevoObj.address"></th>
            <th><input type="text" v-model="contactoNuevoObj.phone"></th>
            <th><input type="text" v-model="contactoNuevoObj.country"></th>
            <th><input type="text" v-model="contactoNuevoObj.city"></th>
            <th><button @click="guardarNuevo()">Agregar</button></th>
          </tr> 
          <tr v-if="indexParaEditar !== null">
            <th></th>
            <th><input type="text" v-model="contactoEditarObj.nombre"></th>
            <th><input type="text" v-model="contactoEditarObj.email"></th>
            <th><input type="text" v-model="contactoEditarObj.address"></th>
            <th><input type="text" v-model="contactoEditarObj.phone"></th>
            <th><input type="text" v-model="contactoEditarObj.country"></th>
            <th><input type="text" v-model="contactoEditarObj.city"></th>
            <th><button @click="guardarEdicion()">Guardar</button></th>
          </tr> 
        </thead>
        <tbody>
          <tr v-for="(contacto, index) in displayedContactos" :key="contacto.id">
            <td>{{ contacto.id }}</td>
            <td>{{ contacto.nombre }}</td>
            <td>{{ contacto.email }}</td>
            <td>{{ contacto.address }}</td>
            <td>{{ contacto.phone }}</td>
            <td>{{ contacto.country }}</td>
            <td>{{ contacto.city }}</td>
            <td><button @click="EditarContacto(contacto, index)">Editar</button><button @click="eliminarContacto(index)">Eliminar</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    name: 'MiComponente',
    data() {
      return {
        message: 'Lista de Contactos',
        indexParaEditar: null,
        searchQuery: '',
        contactoNuevoObj: {
          nombre: '',
          email: '',
          address: '',
          phone: '',
          country: '',
          city: ''
        },
        contactoEditarObj: {
          nombre: '',
          email: '',
          address: '',
          phone: '',
          country: '',
          city: ''
        },
        contactos: [
          {id: "1", nombre: "John Doe", email: "john.doe@example.com", address: "123 Main St, Anytown USA", phone: "555-555-5555", country: "USA", city: "Anytown"},
          {id: "2", nombre: "Jane Smith", email: "jane.smith@example.com", address: "456 Oak St, Anytown USA", phone: "555-555-5555", country: "USA", city: "Anytown"},
          {id: "3", nombre: "Bob Johnson", email: "bob.johnson@example.com", address: "789 Pine St, Anytown USA", phone: "555-555-5555", country: "USA", city: "Anytown"},
          {id: "4", nombre: "Alice Brown", email: "alice.brown@example.com", address: "321 Elm St, Anytown USA", phone: "555-555-5555", country: "USA", city: "Anytown"},
          {id: "5", nombre: "Mike Davis", email: "mike.davis@example.com", address: "654 Maple St, Anytown USA", phone: "555-555-5555", country: "USA", city: "Anytown"},
          {id: "6", nombre: "Sarah Wilson", email: "sarah.wilson@example.com", address: "987 Cedar St, Anytown USA", phone: "555-555-5555", country: "USA", city: "Anytown"},
          {id: "7", nombre: "Tom Wilson", email: "tom.wilson@example.com", address: "987 Cedar St, Anytown USA", phone: "555-555-5555", country: "USA", city: "Anytown"},
          {id: "8", nombre: "Sarah Wilson", email: "sarah.wilson@example.com", address: "987 Cedar St, Anytown USA", phone: "555-555-5555", country: "USA", city: "Anytown"}
        ],
        resultadoBusqueda: []
      }
    },
    components: {
        // Registro de componentes que se utilizaran.
    },
    methods: {
        // métodos que se pueden llamar desde la plantilla o desde otras partes del componente.
        guardarNuevo(){
          const nuevoContacto = {...this.contactoNuevoObj, id: this.nextId};
          this.contactos.push(nuevoContacto);
          this.resetNuevoContacto();
          // this.contactos.push(Object.assign({},this.contactoNuevoObj)); 
        },
        eliminarContacto(index){
          this.contactos.splice(index, 1);
        },
        guardarEdicion(){
          this.contactos[this.indexParaEditar] = Object.assign({},this.contactoEditarObj);
          this.indexParaEditar = null;
        },
        EditarContacto(contacto, index){
          this.indexParaEditar = index;
          this.contactoEditarObj = Object.assign({},contacto);
        },
        updateSearchQuery(event) {
          this.searchQuery = event.target.value;
        },
        buscarContacto(){
          const query = this.searchQuery.toLowerCase();
          this.resultadoBusqueda = this.contactos.filter(contacto =>{
            return contacto.nombre.toLowerCase().includes(query) || contacto.email.toLowerCase().includes(query);
          });
        },
        resetNuevoContacto() {
          this.contactoNuevoObj = {
            nombre: '',
            email: '',
            address: '',
            phone: '',
            country: '',
            city: ''
          };
        },
    },
    computed: {
      // propiedades computadas que dependen de otras propiedades reactivas
      nextId() {
        const maxId = this.contactos.reduce((max, contacto) => Math.max(max, parseInt(contacto.id)), 0);
        return (maxId + 1).toString();
      },
      displayedContactos() {
        return this.resultadoBusqueda.length ? this.resultadoBusqueda : this.contactos;
      }
    },
    props: {
        // propiedades que el componente puede recibir.
    },
    emits: [] // los eventos personalizados que el componente puede emitir.
  }
  </script>
  
  <style>
  h1 {
    color: #42b983;
  }
  table {
    width: 100%;
    border-collapse: collapse;
  }
  th, td {
    border: 1px solid #ddd;
    padding: 8px;
  }
  th {
    background-color: #f2f2f2;
    text-align: center;
  }
  .search-container {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
  }
  .search-box {
    width: 300px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  .search-button {
    padding: 10px 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f0f0f0;
    cursor: pointer;
    margin-left: 10px;
  }
  </style>
  