<template>
  <main v-if="!loading">
    <DataTitle :text='title' :dataDate='dataDate'/>
    <DataBoxes :stats='stats'/>
    <CountrySelect @get-country="getCountryData" :countries='countries'/>
  </main>

</template>

<script>
import DataTitle from '@/components/DataTitle';
import DataBoxes from '@/components/DataBoxes';
import CountrySelect from '@/components/CountrySelect';


export default {
  name: 'Home',
  components:{
    DataTitle,
    DataBoxes,
    CountrySelect
    
  },
  data(){
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: []
    }
  },
  methods:{
    async fetchCovidData(){
      const res = await fetch('https://api.covid19api.com/summary');
      const data = await res.json();
      return data;
    },
    getCountryData(country){
      this.title = country.Country;
      this.stats = country;
      console.log(country)
    }
  },
  async created(){
    const data = await this.fetchCovidData();
    console.log(data);
    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  }
}
</script>