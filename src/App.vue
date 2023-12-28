<template lang="pug">
v-app
  v-app-bar(app color="purple" dark)
    v-toolbar-title(class="text-h6 font-weight-medium text-center")
      span.text-center Nunes Sports
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
