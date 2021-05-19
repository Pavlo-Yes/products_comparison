<template>
  <table>
    <tr>
      <!--    Products header row(start) -->
      <td>
        <label>Показати лише відмінності</label>
        <input type="checkbox" v-model="showDiff">
      </td>
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
    <template v-if="!showDiff">
      <!--      Default product characteristics (start) -->
      <tr v-for="char in characteristics" :key="char.id">
        <td>
          {{ char.characteristics_property_i18_n.name | toUppercase }}
        </td>
        <template v-for="(form, i) in getPropertys">
          <td :key="i" v-if="form[char.id]">
            {{ form[char.id].value | toUppercase}}
          </td>
          <td :key="i" v-if="!form[char.id]">-</td>
        </template>
      </tr>
      <!--      Default product characteristics (end) -->
    </template>
    <!--    Product show only difference characteristics (start) -->
    <template v-if="showDiff">
      <tr v-for="(char, i) in getOnlyDiff" :key="i">
        <td>{{ char.name | toUppercase }}</td>
        <template v-for="(item, index) in char.value">
          <td :key="index" v-if="!item.characteristics_default_values_i18_n">
            {{ item.value }}
          </td>
          <td :key="index" v-else-if="!item.value">
            {{ item.characteristics_default_values_i18_n.name  | toUppercase }}
          </td>
          <td :key="index" v-if="!item">-</td>
        </template>
      </tr>
    </template>
    <!--    Product show only difference characteristics (end) -->
    </tbody>
  </table>
</template>

<script>
export default {
  name: "Products",
  data() {
    return {
      showDiff: false,
      products: require('../../products.json'),
      characteristics: require('../../categories.json')
    }
  },
  computed: {
    getOnlyDiff() {
      const parsedCharacteristics = [];
      for (let i = 0; i < this.products.length; i++) {
        const singleProduct = this.products[i];
        singleProduct.characteristics_values_for_product.map(charValForProd => {
          charValForProd.characteristics_default_values.map(charDefVal => {
            charDefVal.pr_id = charValForProd.id;
            parsedCharacteristics.push(charDefVal);
          })
        })
      }

      let parsedCharObj = {};
      for (let i = 0; i < parsedCharacteristics.length; i++) {
        const singleParsedChar = parsedCharacteristics[i];
        let characteristic = this.characteristics.find(singleOriginalChar => {
          return singleOriginalChar.id === singleParsedChar.characteristics_property_id;
        });
        let alias = characteristic.characteristics_property_i18_n.name;
        //adding array of char-c to parsedObj with key=alias
        if (!Object.prototype.hasOwnProperty.call(parsedCharObj, alias)) {
          parsedCharObj[alias] = [];
          parsedCharObj[alias].push(singleParsedChar);
        } else {
          parsedCharObj[alias].push(singleParsedChar);
        }
      }

      let notEqualCharObj = {};
      for (const key in parsedCharObj) {
        const singleParsedCharObj = parsedCharObj[key];
        if (Object.hasOwnProperty.call(parsedCharObj, key)) {
          //key-char-c name, parsedCharObj-array with char-cs
          singleParsedCharObj.filter((singleChar) => {
            if (!singleChar.value && !singleChar.characteristics_default_values_i18_n) {
              return;
            }
            if (!singleChar.value && singleChar.characteristics_default_values_i18_n.name
                !== singleParsedCharObj[0].characteristics_default_values_i18_n.name) {
              notEqualCharObj[key] = {name: key, value: singleParsedCharObj}
            }
            if (singleChar.value !== singleParsedCharObj[0].value) {
              notEqualCharObj[key] = {name: key, value: singleParsedCharObj}
            }
          });
        }
      }
      console.log(notEqualCharObj);
      return notEqualCharObj
    },
    getPropertys: function () {
      const result = [];
      this.products.map(product => {
        product.characteristics_values_for_product.map(charValForProduct => {
          charValForProduct.characteristics_default_values.map(charDefVal => {
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 14) {
              charValForProduct[charDefVal.characteristics_property_id] =
                  {value: charDefVal.characteristics_default_values_i18_n.name}
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 13) {
              charValForProduct[charDefVal.characteristics_property_id] =
                  {value: charDefVal.characteristics_default_values_i18_n.name}
            }
            if (charDefVal.value !== null &&
                charDefVal.characteristics_property_id === 11) {
              charValForProduct[charDefVal.characteristics_property_id] = {value: charDefVal.value}
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 10) {
              charValForProduct[charDefVal.characteristics_property_id] =
                  {value: charDefVal.characteristics_default_values_i18_n.name}
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 9) {
              charValForProduct[charDefVal.characteristics_property_id] =
                  {value: charDefVal.characteristics_default_values_i18_n.name}
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 8) {
              charValForProduct[charDefVal.characteristics_property_id] =
                  {value: charDefVal.characteristics_default_values_i18_n.name}
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 7) {
              charValForProduct[charDefVal.characteristics_property_id] =
                  {value: charDefVal.characteristics_default_values_i18_n.name}
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 6) {
              charValForProduct[charDefVal.characteristics_property_id] =
                  {value: charDefVal.characteristics_default_values_i18_n.name}
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 5) {
              charValForProduct[charDefVal.characteristics_property_id] =
                  {value: charDefVal.characteristics_default_values_i18_n.name}
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 4) {
              charValForProduct[charDefVal.characteristics_property_id] =
                  {value: charDefVal.characteristics_default_values_i18_n.name}
            }
            if (charDefVal.value !== null &&
                charDefVal.characteristics_property_id === 3) {
              charValForProduct[charDefVal.characteristics_property_id] = {value: charDefVal.value}
            }
          })
          result.push(charValForProduct)
        })
      })
      return result
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
  margin: 20px;
  max-width: 800px;
  background-color: #e3e3c8;
  border-collapse: collapse;
}

td {
  border: 1px solid gray;
  padding: 10px
}

tbody td {
  height: 20px;
}

tr :first-child {
  font-weight: bold;
  width: 120px;
}

.price {
  font-weight: bold;
}

img {
  margin: 0 auto;
  display: block;
}
</style>