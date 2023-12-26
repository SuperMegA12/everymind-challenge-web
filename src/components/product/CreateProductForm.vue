<template lang="pug">
v-container
  v-form(ref="form" v-model="valid" lazy-validation)
    v-row
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="name" label="Name" required)
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="description" label="Description" required)
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="code" label="Code" required)
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="price" label="Price" required type="number")
      v-col
        v-btn(color="primary" @click="createProduct" :disabled="!valid") Create Product
</template>

<script>
import axios from 'axios';

export default {
  data: () => ({
    name: '',
    description: '',
    code: '',
    price: null,
    valid: false,
  }),

  methods: {
    createProduct() {
      const newProduct = {
        name: this.name,
        description: this.description,
        code: this.code,
        price: this.price,
      };

      axios.post('http://localhost:8080/products/create', newProduct)
        .then(response => {
          console.log('Product created successfully:', response.data);
          this.$refs.form.reset();
        })
        .catch(error => {
          console.error('Error creating product:', error);
        });
    },
  },
};
</script>
