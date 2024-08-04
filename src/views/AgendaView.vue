<template>
  <div class="container">
    <h1>{{ titulo }}</h1>
    
    <!-- Campo de búsqueda -->
    <div class="search-wrapper">
      <input type="text" v-model="searchQuery" placeholder="Buscar por nombre o correo electrónico">
    </div>

    <div class="table-wrapper">
      <table>
        <thead>
          <tr>
            <th class="fixed-id">ID</th>
            <th>Name</th>
            <th>Email</th>
            <th>Address</th>
            <th>Phone</th>
            <th>Country</th>
            <th>City</th>
            <th></th>
            <th></th>
          </tr>
          <tr>
            <th class="fixed-id"><input type="text" v-model="contactoNuevoObj.id"></th>
            <th><input type="text" v-model="contactoNuevoObj.name"></th>
            <th><input type="text" v-model="contactoNuevoObj.email"></th>
            <th><input type="text" v-model="contactoNuevoObj.address"></th>
            <th><input type="text" v-model="contactoNuevoObj.phone"></th>
            <th><input type="text" v-model="contactoNuevoObj.country"></th>
            <th><input type="text" v-model="contactoNuevoObj.city"></th>
            <th><button @click="guardarNuevo()">Nuevo</button></th>
            <th></th>
          </tr>
          <tr v-if="indexParaEditar !== null">
            <th><input type="text" v-model="contactoEditarObj.id"></th>
            <th><input type="text" v-model="contactoEditarObj.name"></th>
            <th><input type="text" v-model="contactoEditarObj.email"></th>
            <th><input type="text" v-model="contactoEditarObj.address"></th>
            <th><input type="text" v-model="contactoEditarObj.phone"></th>
            <th><input type="text" v-model="contactoEditarObj.country"></th>
            <th><input type="text" v-model="contactoEditarObj.city"></th>
            <th><button @click="guardarEdicion()">Guardar</button></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(contacto, index) in contactosFiltrados" :key="contacto.id">
            <td class="fixed-id">{{ contacto.id }}</td>
            <td>{{ contacto.name }}</td>
            <td>{{ contacto.email }}</td>
            <td>{{ contacto.address }}</td>
            <td>{{ contacto.phone }}</td>
            <td>{{ contacto.country }}</td>
            <td>{{ contacto.city }}</td>
            <td><button @click="eliminarContacto(index)">Eliminar</button></td>
            <td><button @click="editarContacto(contacto, index)">Editar</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
export default {
  name: 'MiComponente',
  data() {
    return {
      titulo: 'Registro de Contactos',
      searchQuery: '', // Campo para la búsqueda
      contactoNuevoObj: {
        id: "",
        name: "",
        email: "",
        address: "",
        phone: "",
        country: "",
        city: ""
      },
      contactoEditarObj: {
        id: "",
        name: "",
        email: "",
        address: "",
        phone: "",
        country: "",
        city: ""
      },
      indexParaEditar: null,
      contactos: [
        {
          id: 1,
          name: "Alice Johnson",
          email: "alice.johnson@example.com",
          address: "123 Maple Street",
          phone: "123-456-7890",
          country: "USA",
          city: "New York"
        },
        {
          id: 2,
          name: "Bob Smith",
          email: "bob.smith@example.com",
          address: "456 Oak Avenue",
          phone: "987-654-3210",
          country: "Canada",
          city: "Toronto"
        },
        {
          id: 3,
          name: "Carol White",
          email: "carol.white@example.com",
          address: "789 Pine Road",
          phone: "555-123-4567",
          country: "UK",
          city: "London"
        },
        {
          id: 4,
          name: "David Brown",
          email: "david.brown@example.com",
          address: "321 Elm Street",
          phone: "444-555-6666",
          country: "Australia",
          city: "Sydney"
        },
        {
          id: 5,
          name: "Emily Davis",
          email: "emily.davis@example.com",
          address: "654 Spruce Lane",
          phone: "333-444-5555",
          country: "USA",
          city: "Los Angeles"
        }
      ]
    }
  },
  computed: {
    contactosFiltrados() {
      return this.contactos.filter(contacto => {
        return contacto.name.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
               contacto.email.toLowerCase().includes(this.searchQuery.toLowerCase());
      });
    }
  },
  methods: {
    guardarNuevo() {
      this.contactos.push(Object.assign({}, this.contactoNuevoObj));
      this.contactoNuevoObj = {};
    },
    eliminarContacto(index) {
      this.contactos.splice(index, 1);
    },
    guardarEdicion() {
      this.contactos.splice(this.indexParaEditar, 1, this.contactoEditarObj);
      this.contactoEditarObj = {};
      this.indexParaEditar = null;
    },
    editarContacto(contacto, index) {
      this.indexParaEditar = index;
      this.contactoEditarObj = Object.assign({}, contacto);
    }
  }
}
</script>
<style>
/* General */
body, html {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  font-family: Arial, sans-serif;
  box-sizing: border-box;
}

/* Encabezado */
h1 {
  color: #42b983;
  text-align: center;
  margin-top: 20px;
}

/* Contenedor */
.container {
  margin: 0 auto;
  padding: 20px;
  max-width: 1200px;
}

/* Campo de búsqueda */
.search-wrapper {
  margin-bottom: 20px;
  text-align: center;
}

.search-wrapper input {
  padding: 10px;
  width: 80%;
  max-width: 400px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

/* Tabla */
.table-wrapper {
  width: 100%;
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  table-layout: fixed;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: center;
  word-wrap: break-word;
}

th {
  background-color: #42b983;
  color: white;
}

.fixed-id {
  width: 50px;
}

/* Responsividad */
@media screen and (max-width: 600px) {
  table, thead, tbody, th, td, tr {
    display: block;
  }

  thead {
    display: none;
  }

  tr {
    display: flex;
    flex-direction: column;
    border: 1px solid #ddd;
    margin-bottom: 10px;
  }

  td {
    border: none;
    padding: 10px;
    position: relative;
    padding-left: 50%;
  }

  td:before {
    content: attr(data-label);
    font-weight: bold;
    width: 45%;
    display: inline-block;
    position: absolute;
    left: 10px;
    white-space: nowrap;
  }
}

/* Mejoras visuales */
th, td {
  transition: background-color 0.3s ease;
}

tr:hover td {
  background-color: #f9f9f9;
}
</style>
