<template>
  <div>
    <b-input
        v-model="targetCountry"
    />
    <b-btn id="search-btn" @click="findCountry"> search!</b-btn>
    <b-table
        striped hover
        :items="countryList"
        @row-clicked="openModal"
    >
    </b-table>
    <b-modal v-model="isModalOpen" title="BootstrapVue">
      {{ selectedCountry.name }}
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