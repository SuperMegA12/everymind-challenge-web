<template lang="pug">
v-container
  v-data-table(:headers="headers" :items="products" :search="search")
    template(v-slot:top)
      v-row
        v-col
          v-text-field(v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details)
        v-col(cols="2")
          v-btn(color="primary" @click="openCreateProductModal") Create Product
    template(v-slot:item.actions="{ item }")
      v-icon(v-on:click="deleteProduct(item.id)") mdi-delete

  v-dialog(v-model="isCreateProductModalOpen")
    create-product-form(@product-created="closeCreateProductModal")
</template>

<script>
import CreateProductForm from './CreateProductForm'

import axios from 'axios';

export default {

  components: {
    CreateProductForm
  },

  data: () => ({
    products: [],
    search: '',
    isCreateProductModalOpen: false,
    headers: [
      { text: 'ID', value: 'id' },
      { text: 'Name', value: 'name' },
      { text: 'Description', value: 'description' },
      { text: 'Code', value: 'code' },
      { text: 'Price', value: 'price' },
      { text: 'Actions', value: 'actions', sortable: false },
    ],
  }),

  mounted() {
    this.fetchProducts();
  },

  methods: {
    fetchProducts() {
      axios.get('http://localhost:8080/products/list-all')
        .then(response => {
          this.products = response.data;
        })
        .catch(error => {
          console.error('Error fetching products:', error);
        });
    },

    deleteProduct(productId) {
      const confirmDelete = window.confirm('Are you sure you want to delete this product?');
      if (!confirmDelete) return;

      axios.delete(`http://localhost:8080/products/delete/${productId}`)
        .then(response => {
          this.products = this.products.filter(product => product.id !== productId);
          console.log('Product deleted successfully:', response.data);
        })
        .catch(error => {
          console.error('Error deleting product:', error);
        });
    },

    openCreateProductModal() {
      this.isCreateProductModalOpen = true;
    },
  },
};
</script>