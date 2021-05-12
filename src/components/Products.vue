<template>
    <table>
      <tr>
        <!--    Products header row(start) -->
        <td></td>
        <template v-for="(product, ind) in getAllProducts">
          <td :key="ind">
            <img v-if="product.products_photos"
                 :src="product.products_photos.photo"
                 :alt="product.products_i18_n.name"
            >
            <img v-if="!product.products_photos"
                 :src="product.characteristics_variants_for_product_photos[0].photo"
                 :alt="product.nameVyrobnyk"
            >
            <div v-if="product.products_i18_n">
              {{ product.products_i18_n.name }}
            </div>
            <div v-if="!product.products_i18_n">
              Подушка {{ product.nameVyrobnyk }}
              {{ product.nameNapovnyuvach }}
              {{ product.nameRozmir }}
            </div>
            <div class="price">
              {{
                product.characteristics_values_for_product ?
                    product.characteristics_values_for_product[0].price :
                    product.price
              }}
            </div>
          </td>
        </template>
        <!--    Products header row (end)-->
      </tr>

      <tbody>
      <tr>
        <td>ФОРМА</td>
        <template v-for="(forma, i) in getForma">
          <td :key="i">{{ forma.characteristics_default_values_i18_n.name | toUppercase }}</td>
        </template>
      </tr>
      <tr>
        <td>НАПОВНЮВАЧ</td>
        <template v-for="(napovnyuvach, i) in getNapovnyuvach">
          <td :key="i">{{ napovnyuvach.characteristics_default_values_i18_n.name | toUppercase }}</td>
        </template>
      </tr>
      <tr>
        <td>РОЗМІР</td>
        <template v-for="(rozmir, i) in getRozmir">
          <td :key="i">{{ rozmir.value }}</td>
        </template>
      </tr>
      <tr>
        <td>ШИРИНА</td>
        <template v-for="(shyrynaElement, i) in getShyryna">
          <td :key="i" v-if="shyrynaElement.shyryna">
            {{ shyrynaElement.shyryna.value }}
          </td>
          <td :key="i" v-if="!shyrynaElement.shyryna">
            -
          </td>
        </template>
      </tr>
      <tr>
        <td>ЗАСТІБКА НАВОЛОЧКИ</td>
        <template v-for="(zastibkaElement, i) in getZastibka">
          <td :key="i" v-if="zastibkaElement.zastibka">
            {{ zastibkaElement.zastibka.characteristics_default_values_i18_n.name | toUppercase }}
          </td>
          <td :key="i" v-if="!zastibkaElement.zastibka">
            -
          </td>
        </template>
      </tr>
      </tbody>

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
    getNapovnyuvach: function () {
      const napovnyuvach = [];
      this.products.map(product => {
        product.characteristics_values_for_product.map(charValForProduct => {
          charValForProduct.characteristics_default_values.map(charDefVal => {
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 8) {
              napovnyuvach.push(charDefVal)
            }
          })
        })
      })
      return napovnyuvach
    },
    getForma: function () {
      const forma = [];
      this.products.map(product => {
        product.characteristics_values_for_product.map(charValForProduct => {
          charValForProduct.characteristics_default_values.map(charDefVal => {
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 7) {
              forma.push(charDefVal)
            }
          })
        })
      })
      return forma
    },
    getRozmir: function () {
      const rozmir = [];
      this.products.map(product => {
        product.characteristics_values_for_product.map(charValForProduct => {
          charValForProduct.characteristics_default_values.map(charDefVal => {
            if (charDefVal.characteristics_property_id === 3) {
              rozmir.push(charDefVal)
            }
          })
        })
      })
      return rozmir
    },
    getShyryna: function () {
      const shyryna = [];
      this.products.map(product => {
        product.characteristics_values_for_product.map(charValForProduct => {
          charValForProduct.characteristics_default_values.map(charDefVal => {
            if (charDefVal.characteristics_property_id === 11) {
              charValForProduct["shyryna"] = charDefVal
            }
          })
          shyryna.push(charValForProduct)
        })
      })
      return shyryna
    },
    getZastibka: function () {
      const zastibka = [];
      this.products.map(product => {
        product.characteristics_values_for_product.map(charValForProduct => {
          charValForProduct.characteristics_default_values.map(charDefVal => {
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 9) {
              charValForProduct["zastibka"] = charDefVal
            }
          })
          zastibka.push(charValForProduct)
        })
      })
      return zastibka
    },
    getAllProducts: function () {
      const products = [];
      this.products.map(product => {
        if (product.characteristics_values_for_product.length === 1) {
          products.push(product)
        } else {
          product.characteristics_values_for_product.map(charValForProd => {
            charValForProd.characteristics_default_values.map(charDefVal => {
              if (charDefVal.characteristics_property_id === 13) {
                charValForProd["nameVyrobnyk"] = charDefVal.characteristics_default_values_i18_n.name;
              }
              if (charDefVal.characteristics_property_id === 8) {
                charValForProd["nameNapovnyuvach"] = charDefVal.characteristics_default_values_i18_n.name;
              }
              if (charDefVal.characteristics_property_id === 3) {
                charValForProd["nameRozmir"] = charDefVal.value;
              }
            })
            products.push(charValForProd)
          })
        }
      })
      return products
    }
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
table {
  margin: 50px;
  background-color: #e3e3c8;
  border-collapse: collapse;
}
td {
  height: 150px;
  border: 1px solid gray;
  padding: 10px
}
tbody td {
  height: 40px;
}
tr :first-child {
  font-weight: bold;
}
.price {
  font-weight: bold;
}
img {
  margin: 0 auto;
  display: block;
  max-width: 100px;
}
</style>