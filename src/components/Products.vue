<template>
  <table>
    <tr>
      <td class="textSpan">Показати</td>
      <tbody>
      <template v-for="(char, index) in characteristics">
        <td :key="index"
            class="charHead"
            v-if="char.id === 7"
        >
          {{ char.characteristics_property_i18_n.name | toUppercase }}
        </td>
        <td :key="index+1"
            class="charHead"
            v-if="char.id === 8"
        >
          {{ char.characteristics_property_i18_n.name | toUppercase }}
        </td>
        <td :key="index+2"
            class="charHead"
            v-if="char.id === 3"
        >
          {{ char.characteristics_property_i18_n.name | toUppercase }}
        </td>
        <td :key="index+3"
            class="charHead"
            v-if="char.id === 11"
        >
          {{ char.characteristics_property_i18_n.name | toUppercase }}
        </td>
        <td :key="index+4"
            class="charHead"
            v-if="char.id === 9"
        >
          {{ char.characteristics_property_i18_n.name | toUppercase }}
        </td>
      </template>
      </tbody>
    </tr>
    <tr v-for="(product, ind) in products" :key="ind">
      <th>
        <img :src="product.products_photos.photo" :alt="product.products_i18_n.name">
        <div class="head-div">
          {{ product.products_i18_n.name }}
        </div>
        {{ product.characteristics_values_for_product[0].price }}
      </th>
      <tbody>
      <template v-for="(prodChar, i) in
      product.characteristics_values_for_product[0].characteristics_default_values"
      >
        <td :key="i"
            class="td-body"
            v-if="prodChar.characteristics_property_id === 7 &&
            prodChar.characteristics_default_values_i18_n !== null"
        >
          {{ prodChar.characteristics_default_values_i18_n.name | toUppercase }}
        </td>
      </template>
      <template v-for="(prodChar, i) in
      product.characteristics_values_for_product[0].characteristics_default_values"
      >
        <td :key="i"
            class="td-body"
            v-if="prodChar.characteristics_property_id === 8 &&
            prodChar.characteristics_default_values_i18_n !== null"
        >
          {{ prodChar.characteristics_default_values_i18_n.name | toUppercase }}
        </td>
      </template>
      <template v-for="(prodChar, i) in
      product.characteristics_values_for_product[0].characteristics_default_values"
      >
        <td :key="i"
            class="td-body"
            v-if="prodChar.characteristics_property_id === 3 &&
            prodChar.value !== null"
        >
          {{ prodChar.value }}
        </td>
      </template>
      <template v-for="(prodChar, i) in
      product.characteristics_values_for_product[0].characteristics_default_values"
      >
        <td :key="i"
            class="td-body"
            v-if="prodChar.characteristics_property_id === 11 &&
            prodChar.value !== null"
        >
          {{ prodChar.value }}
        </td>
      </template>
      <template v-for="(prodChar, i) in
      product.characteristics_values_for_product[0].characteristics_default_values"
      >
        <td :key="i"
            class="td-body"
            v-if="prodChar.characteristics_property_id === 9 &&
            prodChar.characteristics_default_values_i18_n !== null"
        >
          {{ prodChar.characteristics_default_values_i18_n.name | toUppercase }}
        </td>
      </template>
      </tbody>
    </tr>
  </table>
</template>

<script>
export default {
  name: "Products",
  data() {
    return {
      products: require('../../products.json'),
      characteristics: require('../../categories.json')
    }
  },
  computed: {
    // nonNullProducts: function () {
    //   const prod = []
    //   this.products.map(product => {
    //     product.characteristics_values_for_product.map(charVal => {
    //       charVal.characteristics_default_values.map(charDefVal => {
    //         if (charDefVal.characteristics_default_values_i18_n !== null) {
    //           prod.push(charDefVal)
    //           // return charDefVal
    //         }
    //       })
    //     })
    //   })
    //   console.log(prod);
    //   return prod
    // }
  },
  filters: {
    toUppercase: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.toUpperCase()
    }
  }
}
</script>

<style scoped>

.textSpan {
  height: 150px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.charHead {
  height: 50px;
  margin: 10px;
  font-weight: bolder;
}

.td-body {
  height: 50px;
  margin: 10px;
}

.head-div {
  font-weight: normal;
}

table {
  display: flex;
  margin-top: 50px;
  background-color: beige;
  border: 1px solid gray;
  border-collapse: collapse;
}

th {
  display: flex;
  height: 150px;
  flex-direction: column;
  align-items: center;
}

tr {
  display: flex;
  flex-direction: column;
  border: 1px solid gray;
}

tbody {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border-top: 1px solid gray;
}

img {
  max-width: 100px;
}
</style>