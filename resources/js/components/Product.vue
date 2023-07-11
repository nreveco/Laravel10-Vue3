<template>
    <div>
        <h5 id="associating-form-text-with-form-controls">Nombre:</h5>
        <h6>{{ product.name }}</h6>
        <h5 id="associating-form-text-with-form-controls">Descripción:</h5>
        <p>{{ product.description }}</p>
        <h5 id="associating-form-text-with-form-controls">Precio:</h5>
        <p>Precio: {{ product.price }}</p>
        <router-link :to="`/products/${product.id}/edit`" class="btn btn-primary">Editar</router-link>
    </div>
</template>

<script>
import axios from 'axios';
export default {
data() {
    return {
    product: {}
    }
},
async created() {
    try {
    const response = await axios.get(`/api/products/${this.$route.params.id}`);
    this.product = response.data;
    } catch (error) {
    console.error(error);
    }
}
}
</script>