<template>
  <div>
    <b-input v-model="targetCountry"/>
    <b-btn id="search-btn" @click="findCountry"> search!</b-btn>
    <b-table
        striped hover
        @row-clicked="openModal"
        :fields="tableFields"
        :items="countryList"
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
          this.countryList = response.data
        })
        .catch(function (error) {
          console.error(error);
        })
  },
  data: function () {
    return {
      tableFields: ['name', 'flag'],
      cellFields: ['name', 'capital', 'region', 'subregion', 'population', 'nativeName'],
      countryList: [],
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
    findCountry() {
      // console.log( this.countryList.filter( input => { input === this.country.name}))
    }
  }
}
</script>
<style scoped>

</style>