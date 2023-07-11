<template>
    <div>
      <h2 v-if="isNewProduct">Añadir Producto</h2>
      <h2 v-else>Editar Producto</h2>
        <form @submit.prevent="submitForm">
          <div class="mb-3">
            <label for="name" class="form-label">Nombre:</label>
            <input class="form-control" type="text" id="name" v-model="product.name" required />
          </div>
          <div class="mb-3">
            <label for="description" class="form-label">Descripción:</label>
            <textarea class="form-control" id="description" v-model="product.description" required></textarea>
          </div>
          <div class="mb-3">
            <label for="price" class="form-label">Pricio:</label>
            <input class="form-control" type="number" id="price" v-model="product.price" required />
          </div>
          <button type="submit" v-if="isNewProduct" class="btn btn-primary">Añadir Producto</button>
          <button type="submit" v-else class="btn btn-primary">Modificar Producto</button>
        </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  export default {
    data() {
      return {
        product: {
          name: '',
          description: '',
          price: 0
        }
      }
    },
    computed: {
      isNewProduct() {
        return !this.$route.path.includes('edit');
      }
    },
    async created() {
      if (!this.isNewProduct) {
        const response = await axios.get(`/api/products/${this.$route.params.id}`);
        this.product = response.data;
      }
    },
    methods: {
      async submitForm() {
        try {
          if (this.isNewProduct) {
            await axios.post('/api/products', this.product);
          } else {
            await axios.put(`/api/products/${this.$route.params.id}`, this.product);
          }
          this.$router.push('/');
        } catch (error) {
          console.error(error);
        }
      }
    }
  }
  </script>