<template lang="pug">
v-app
  v-main
    ProductTable
</template>

<script>
import ProductTable from './components/product/ProductTable'

export default {
  components: {
    ProductTable
  },

  data: () => ({
    //
  }),
};
</script>
