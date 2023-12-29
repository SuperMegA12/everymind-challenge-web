<template lang="pug">
v-app
  v-app-bar(app color="purple" dark)
    v-col(class="d-flex justify-space-around")
      v-toolbar-title(class="text-h6 font-weight-medium text-center") Nunes Sports Web
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
