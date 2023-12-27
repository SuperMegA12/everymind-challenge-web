<template lang="pug">
v-container
  v-form(ref="form" v-model="valid")
    v-row
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="name" label="Name" :rules="[v => !!v || 'Name is required']")
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="description" label="Description" :rules="[v => !!v || 'Description is required']")
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="code" label="Code" type="number" :rules="[v => !!v || 'Code is required']" outlined inputmode="numeric")
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="price" label="Price" type="number" :rules="[v => !!v || 'Price is required']" outlined inputmode="numeric")
      v-col
        v-btn(color="primary" @click="createProduct" :disabled="!valid") Confirm
  v-snackbar(v-model="snackbar.show" :color="snackbar.color" :timeout="snackbar.timeout" bottom)
    span {{ snackbar.text }}
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
    snackbar: {
      show: false,
      text: '',
      color: '',
      timeout: 6000, // milliseconds
    },
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
          this.showSnackbar('Product created successfully', 'primary');
          this.$emit('product-created');
          this.cleanFields();
          console.log(response.data);
        })
        .catch(error => {
          this.showSnackbar('Error creating product', 'error');
          this.cleanFields();
          console.error('Error creating product:', error);
        });
    },

    showSnackbar(text, color) {
      this.snackbar.text = text;
      this.snackbar.color = color;
      this.snackbar.show = true;

      setTimeout(() => {
        this.snackbar.show = false;
      }, this.snackbar.timeout);
    },

    cleanFields() {
      this.name = '';
      this.description = '';
      this.code = '';
      this.price = null;
      this.valid = false;
    },
  },
};
</script>
