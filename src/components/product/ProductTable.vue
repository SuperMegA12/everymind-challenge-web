<template lang="pug">
v-container
  v-data-table(:headers="headers" :items="filteredProducts" :search="search")
    template(v-slot:top)
      v-row
        v-col
          v-text-field(v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details)
    template(v-slot:item.actions="{ item }")
      v-icon(v-on:click="deleteProduct(item.id)") mdi-delete
      v-icon(v-on:click="openUpdateProductModal(item.id, item.name, item.description, item.code, item.price)") mdi-pencil
  v-col(cols="2")
    v-btn(color="primary" @click="openCreateProductModal") Create Product

  v-dialog(v-model="isCreateProductModalOpen")
    v-card
      create-product-form(@product-created="fetchProducts")
  v-dialog(v-model="isUpdateProductModalOpen")
    v-card
      update-product-form(
        :product-id="updatingProductId"
        :existing-name="updatingProductName"
        :existing-description="updatingProductDescription"
        :existing-code="updatingProductCode"
        :existing-price="updatingProductPrice"
        @product-updated="fetchProducts"
      )

  v-snackbar(v-model="snackbar.show" :color="snackbar.color" :timeout="snackbar.timeout" bottom)
    span {{ snackbar.text }}
</template>

<script>
import CreateProductForm from './CreateProductForm';
import UpdateProductForm from './UpdateProductForm';

import axios from 'axios';

export default {
  components: {
    CreateProductForm,
    UpdateProductForm
  },

  data: () => ({
    products: [],
    search: '',
    isCreateProductModalOpen: false,
    isUpdateProductModalOpen: false,
    updatingProductId: null,
    updatingProductName: '',
    updatingProductDescription: '',
    updatingProductCode: null,
    updatingProductPrice: null,
    snackbar: {
      show: false,
      text: '',
      color: '',
      timeout: 6000,
    },
    headers: [
      { text: 'ID', value: 'id' },
      { text: 'Name', value: 'name' },
      { text: 'Description', value: 'description' },
      { text: 'Code', value: 'code' },
      { text: 'Price', value: 'price' },
      { text: 'Actions', value: 'actions', sortable: false },
    ],
  }),

  computed: {
    filteredProducts() {
      return this.products.filter(product =>
        product.name.toLowerCase().includes(this.search.toLowerCase()) ||
        product.description.toLowerCase().includes(this.search.toLowerCase()) ||
        String(product.code).includes(this.search) ||
        String(product.price).includes(this.search)
      );
    },
  },

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
          this.showSnackbar('Error fetching products ' + error, 'error');
          console.error('Error fetching products:', error);
        });

      this.closeCreateProductModal();
      this.closeUpdateProductModal();
    },

    deleteProduct(productId) {
      const confirmDelete = window.confirm('Are you sure you want to delete this product?');
      if (!confirmDelete) return;

      axios.delete(`http://localhost:8080/products/delete/${productId}`)
        .then(response => {
          this.products = this.products.filter(product => product.id !== productId);
          this.showSnackbar(response.data, 'primary');
          console.log(response.data);
        })
        .catch(error => {
          this.showSnackbar('Error deleting product', 'error');
          console.error('Error deleting product:', error);
        });
    },

    openCreateProductModal() {
      this.isCreateProductModalOpen = true;
    },

    openUpdateProductModal(productId, name, description, code, price) {
      this.updatingProductId = productId;
      this.updatingProductName = name;
      this.updatingProductDescription = description;
      this.updatingProductCode = code;
      this.updatingProductPrice = price;
      this.isUpdateProductModalOpen = true;
    },

    closeCreateProductModal() {
      this.isCreateProductModalOpen = false;
    },

    closeUpdateProductModal() {
      this.isUpdateProductModalOpen = false;
    },

    showSnackbar(text, color) {
      this.snackbar.text = text;
      this.snackbar.color = color;
      this.snackbar.show = true;

      setTimeout(() => {
        this.snackbar.show = false;
      }, this.snackbar.timeout);
    },
  },
};
</script>
