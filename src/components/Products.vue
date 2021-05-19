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
          <td :key="i"
              v-if="form.forma && char.id===form.forma.characteristics_property_id">
            {{ form.forma.characteristics_default_values_i18_n.name | toUppercase }}
          </td>
          <td :key="i" v-if="!form.forma && char.id===7">-</td>
        </template>
        <template v-for="(napov, i) in getPropertys">
          <td :key="i"
              v-if="napov.napovnyuvach && char.id===napov.napovnyuvach.characteristics_property_id">
            {{ napov.napovnyuvach.characteristics_default_values_i18_n.name | toUppercase }}
          </td>
          <td :key="i" v-if="!napov.napovnyuvach && char.id===8">-</td>
        </template>
        <template v-for="(rozm, i) in getPropertys">
          <td :key="i"
              v-if="rozm.rozmir && char.id===rozm.rozmir.characteristics_property_id">
            {{ rozm.rozmir.value }}
          </td>
          <td :key="i" v-if="!rozm.rozmir && char.id===3">-</td>
        </template>
        <template v-for="(shyr, i) in getPropertys">
          <td :key="i"
              v-if="shyr.shyryna && char.id===shyr.shyryna.characteristics_property_id">
            {{ shyr.shyryna.value }}
          </td>
          <td :key="i" v-if="!shyr.shyryna && char.id===11">-</td>
        </template>
        <template v-for="(zastibkaElement, i) in getPropertys">
          <td :key="i"
              v-if="zastibkaElement.zastibka && char.id===zastibkaElement.zastibka.characteristics_property_id">
            {{ zastibkaElement.zastibka.characteristics_default_values_i18_n.name | toUppercase }}
          </td>
          <td :key="i" v-if="!zastibkaElement.zastibka && char.id===9">-</td>
        </template>
        <template v-for="(gabaryt, i) in getPropertys">
          <td :key="i"
              v-if="gabaryt.gabaryty && char.id===gabaryt.gabaryty.characteristics_property_id">
            {{ gabaryt.gabaryty.characteristics_default_values_i18_n.name | toUppercase }}
          </td>
          <td :key="i" v-if="!gabaryt.gabaryty && char.id===4">-</td>
        </template>
        <template v-for="(typ, i) in getPropertys">
          <td :key="i"
              v-if="typ.typ && char.id===typ.typ.characteristics_property_id">
            {{ typ.typ.characteristics_default_values_i18_n.name | toUppercase }}
          </td>
          <td :key="i" v-if="!typ.typ && char.id===5">-</td>
        </template>
        <template v-for="(dovzhyna, i) in getPropertys">
          <td :key="i"
              v-if="dovzhyna.dovzhyna && char.id===dovzhyna.dovzhyna.characteristics_property_id">
            {{ dovzhyna.dovzhyna.value | toUppercase }}
          </td>
          <td :key="i" v-if="!dovzhyna.dovzhyna && char.id===6">-</td>
        </template>
        <template v-for="(kol, i) in getPropertys">
          <td :key="i"
              v-if="kol.kolir && char.id===kol.kolir.characteristics_property_id">
            {{ kol.kolir.characteristics_default_values_i18_n.name | toUppercase }}
          </td>
          <td :key="i" v-if="!kol.kolir && char.id===14">-</td>
        </template>
        <template v-for="(vyrobnyk, i) in getPropertys">
          <td :key="i"
              v-if="vyrobnyk.kraina && char.id===vyrobnyk.kraina.characteristics_property_id">
            {{ vyrobnyk.kraina.characteristics_default_values_i18_n.name | toUppercase }}
          </td>
          <td :key="i" v-if="!vyrobnyk.kraina && char.id===10">-</td>
        </template>
        <template v-for="(vyrob, i) in getPropertys">
          <td :key="i"
              v-if="vyrob.vyrobnyk && char.id===vyrob.vyrobnyk.characteristics_property_id">
            {{ vyrob.vyrobnyk.characteristics_default_values_i18_n.name | toUppercase }}
          </td>
          <td :key="i" v-if="!vyrob.vyrobnyk && char.id===13">-</td>
        </template>
      </tr>
      <!--      Default product characteristics (end) -->
    </template>
    <!--    Product show only difference characteristics (start) -->
    <template v-if="showDiff">
      <tr v-for="(char, i) in getOnlyDiff" :key="i">
        <td>{{ char.name }}</td>
        <template v-for="(item, index) in char.value">
          <td :key="index" v-if="!item.characteristics_default_values_i18_n">
            {{ item.value }}
          </td>
          <td :key="index" v-else-if="!item.value">
            {{ item.characteristics_default_values_i18_n.name  | toUppercase}}
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
      return notEqualCharObj
    },
    getPropertys: function () {
      const result = [];
      this.products.map(product => {
        product.characteristics_values_for_product.map(charValForProduct => {
          charValForProduct.characteristics_default_values.map(charDefVal => {
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 14) {
              charValForProduct["kolir"] = charDefVal
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 13) {
              charValForProduct["vyrobnyk"] = charDefVal
            }
            if (charDefVal.value !== null &&
                charDefVal.characteristics_property_id === 11) {
              charValForProduct["shyryna"] = charDefVal
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 10) {
              charValForProduct["kraina"] = charDefVal
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 9) {
              charValForProduct["zastibka"] = charDefVal
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 8) {
              charValForProduct["napovnyuvach"] = charDefVal
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 7) {
              charValForProduct["forma"] = charDefVal
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 6) {
              charValForProduct["dovzhyna"] = charDefVal
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 5) {
              charValForProduct["typ"] = charDefVal
            }
            if (charDefVal.characteristics_default_values_i18_n !== null &&
                charDefVal.characteristics_property_id === 4) {
              charValForProduct["gabaryty"] = charDefVal
            }
            if (charDefVal.value !== null &&
                charDefVal.characteristics_property_id === 3) {
              charValForProduct["rozmir"] = charDefVal
            }
          })
          result.push(charValForProduct)
        })
      })
      console.log(result);
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
  /*height: 100px;*/
  border: 1px solid gray;
  padding: 10px
}

tbody td {
  height: 20px;
}

tr :first-child {
  font-weight: bold;
  max-width: 100px;
}

.price {
  font-weight: bold;
}

img {
  margin: 0 auto;
  display: block;
}
</style>