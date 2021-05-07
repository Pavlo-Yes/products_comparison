<template>
  <div>
    <button @click="showProducts=!showProducts">Show Products</button>
    <div v-if="showProducts">
      <Products/>
    </div>
  </div>
</template>

<script>
import Products from "./components/Products";

export default {
  name: 'App',
  components: {
    Products
  },
  data() {
    return {
      showProducts: true
    }
  }
}
</script>