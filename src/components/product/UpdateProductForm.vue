<template lang="pug">
v-container
  v-form(ref="form" v-model="valid")
    v-row
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="updatedName" :value="existingName" label="Name")
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="updatedDescription" :value="existingDescription" label="Description")
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="updatedCode" :value="existingCode" label="Code" type="number" hint="Code must be unique" outlined inputmode="numeric")
      v-col(cols="12" sm="6" md="4")
        v-text-field(v-model="updatedPrice" :value="existingPrice" label="Price" type="number" outlined inputmode="numeric")
      v-col
        v-btn(color="primary" @click="updateProduct" :disabled="!valid") Update
</template>

<script>
import axios from 'axios';

export default {
  props: {
    productId: {
      type: Number,
      required: true,
    },
    existingName: String,
    existingDescription: String,
    existingCode: Number,
    existingPrice: Number,
  },

  data: () => ({
    updatedName: '',
    updatedDescription: '',
    updatedCode: '',
    updatedPrice: null,
    valid: false,
  }),

  mounted() {
    this.updatedName = this.existingName || '';
    this.updatedDescription = this.existingDescription || '';
    this.updatedCode = this.existingCode || null;
    this.updatedPrice = this.existingPrice || null;
  },

  methods: {
    updateProduct() {
      const updatedProduct = {
        name: this.updatedName,
        description: this.updatedDescription,
        code: this.updatedCode,
        price: this.updatedPrice,
      };

      axios.put(`http://localhost:8080/products/update/${this.productId}`, updatedProduct)
        .then(response => {
          this.showSnackbar('Product updated successfully', 'primary');
          this.$emit('product-updated');
          this.cleanFields();
          console.log(response.data);
        })
        .catch(error => {
          this.showSnackbar('Error updating product ' + error, 'error');
          this.cleanFields();
          console.error('Error updating product:', error);
        });
    },

    showSnackbar(text, color) {
      this.$emit('show-snackbar', text, color);
    },

    cleanFields() {
      this.updatedName = '';
      this.updatedDescription = '';
      this.updatedCode = '';
      this.updatedPrice = null;
      this.valid = false;
    },
  },
};
</script>
