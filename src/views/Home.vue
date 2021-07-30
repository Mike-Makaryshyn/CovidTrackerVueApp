<template>
  <div>
    <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats"/>
    <CountrySelect 
    :countries="countries"
    @get-country="getCoutryData"/>

    <button  
    v-show="stats.Country"
    @click="showGlobal"
    class="bg-green-700 text-wtite rounded p-3 mt-10 focus:outline-none hove:bg-green-600">
      Clear Coutry
    </button>
    </main>

    <main v-else class="flex flex-col align-center justify-center text-center" >
      <div class="text-gray-500 text-3xl mt-10 mb-6">
        Fetching data
      </div>
      <img :src="loadingImage" class="w-24 m-auto" alt="img">
    </main>
  </div>
</template>

<script>
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes'
import CountrySelect from '@/components/CountrySelect'
export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
   data() {
    return {
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loading: true,
      loadingImage: require('../assets/hourglass.gif'),
    }
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    },
    getCoutryData(coutry) {
      this.stats = coutry
      this.title = coutry.Country
    },
    async showGlobal() {
      this.loading = true 
      const data = await this.fetchCovidData()
      this.title = 'Global'
      this.stats = data.Global
      this.loading = false 
    }
  },
  async created() {
    const data = await this.fetchCovidData()
    console.log(data);

    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
 }
}
</script>
