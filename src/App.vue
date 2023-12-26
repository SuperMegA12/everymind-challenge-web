<template lang="pug">
v-app
  v-main
    product-table
</template>

<script>
import ProductTable from './components/product/ProductTable'

export default {
  components: {
    ProductTable,
  },
};
</script>
