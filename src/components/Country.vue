<template>
  <div>
    <b-input v-model="targetCountry"/>
    <b-btn id="search-btn" @click="findCountry"> search!</b-btn>
    <b-table
        striped hover
        @row-clicked="openModal"
        v-bind:fields="tableFields"
        v-bind:items="filteredCountryList"
    >
      <template #cell(flag)="{ item }">
        <img v-bind:src="item.flag" v-bind:alt="item.name">
      </template>
    </b-table>
    <b-modal v-model="isModalOpen" v-bind:title="selectedCountry.name">
      <ul>
        <img id="flag-in-the-cell" v-bind:src="selectedCountry.flag" alt="flag picture">
        <li v-for="field in cellFields" :key="field">
          {{ field }} : {{ selectedCountry[field] }}
        </li>
      </ul>
    </b-modal>
  </div>
</template>

<script>
import axios from "axios"

export default {
  name: "Country",
  created() {
    axios.get('https://restcountries.eu/rest/v2/all')
        .then((response) => {
          // this.countryList = response.data
          this.backUpCountryList = response.data
        })
        .catch(function (error) {
          console.error(error);
        })
  },
  computed: {
    filteredCountryList: function () {
      let input = this.targetCountry
      return this.backUpCountryList.filter((country) => {
        return !!country.name.includes(input);
      })
    }
  },
  data: function () {
    return {
      tableFields: ['name', 'flag'],
      cellFields: ['name', 'capital', 'region', 'subregion', 'population', 'nativeName'],
      // countryList: [],
      backUpCountryList: [],
      isModalOpen: false,
      selectedCountry: "",
      targetCountry: "",
    }
  },
  methods: {
    openModal(country) {
      this.isModalOpen = true
      this.selectedCountry = country
    },
    // findCountry(input) {
    //   input = this.targetCountry
    //   let possibleCountries = []
    //   this.countryList.forEach( country => {
    //     if( country.name === input) {
    //       possibleCountries.push(country)
    //     }
    //     possibleCountries = this.countryList
    //     console.log(possibleCountries)
    //   })
    // }
  }
}
</script>
<style scoped>

</style>