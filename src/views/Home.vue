<template>
  <div class="main">
    <div class="searchSelect">
      <div class="search-input">

        <i class="icon fas fa-search"></i>
        <i class="x-icon fas fa-times" @click="reset"></i>
        <input class="input-field"
          placeholder="Search for a country"
          v-model="query"
          @keyup="searchCountries(query)"
          @blur="showResults = false"
          @focus="showResults = true"
        >
        
        <div class="result-container" v-if="results.length > 0 && showResults">
          <div class="result" v-for="(result, index) in results" :key="index">
            <div class="result-info">
              <p class="result-name bolder">{{result.name}}</p>
              <p><span class="bold">Capital city:</span> {{result.capital}}</p>
              <p><span class="bold">Native name:</span> {{result.nativeName}}</p>
            </div>
            <img class="result-img" :src="result.flag" alt="flag">
          </div>
          <div class="no-results" v-if="results.length < 1">
            <p>No results for '<span class="bold">{{query}}</span>'</p>
          </div>
        </div>
        
      </div>
      
      <select class="select" @change="filterRegion($event)">
        <option value="0">Filer By Region</option>
        <option value="Africa">Africa</option>
        <option value="Americas">Americas</option>
        <option value="Asia">Asia</option>
        <option value="Europe">Europe</option>
        <option value="Oceania">Oceania</option>
      </select>
    </div>
    <div class="countries">
      <div class="country" v-for="(country, index) in allCountries" :key="index" @click="showDetails(country.name)">
        <img :src="country.flag" alt="flag">
        <div class="country-info">
          <p class="bolder">{{country.name}}</p>
          <p><span class="bold">Population: </span>{{country.population}}</p>
          <p><span class="bold">Region: </span>{{country.region}}</p>
          <p><span class="bold">Capital: </span>{{country.capital}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'

export default {
  name: 'Home',
  setup(){
    // Get all countries
    const allCountries = ref([]);

    const getAllCountries = () => {
      axios.get('https://restcountries.eu/rest/v2/all')
        .then(response => {
          allCountries.value = response.data
        })
    };

    onMounted(getAllCountries);

    // Get details for one country 
    const router = useRouter();

    const showDetails = (country) => {
      router.push({
        name: 'CountryDetails',
        params:{
          country: country.trim()
        }
      })
    }

    // Search countries 
    let showResults = ref(false)
    let query = ref('');

    const searchCountries = (query) => {
      const regex = new RegExp(`${query}`)
      results.value = allCountries.value.filter(country => regex.test(country.name))
    }

    const results = ref([])

    // Filter countries 
    const filterRegion = (event) => {
      axios.get(`https://restcountries.eu/rest/v2/region/${event.target.value.toLowerCase()}`)
        .then(response => {
          allCountries.value = response.data
        })
    }

    const reset = () => {
      query.value = ''
    }

    return {
      allCountries,
      getAllCountries,
      showDetails,
      query,
      filterRegion,
      searchCountries,
      results,
      showResults,
      reset
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../sass/globals";
@import "../sass/mixins";

.main{
  padding: 1em;
  background-color: $very-light-gray;
  color: $very-dark-blue-txt;
  @include breakpoint-up(xlarge){
    max-width: 1350px;
    margin: 0 auto;
  }
  .searchSelect{
    @include breakpoint-up(medium){
      display: flex;
      justify-content: space-between;
    }
    .search-input{
      position: relative;
      width: 300px;
      .icon{
        position: absolute;
        left: 2em;
        top: 1.1em;
        width: 1em;
        color: $dark-gray;
      }
      .x-icon{
        position: absolute;
        right: 0;
        top: 1.1em;
        right: 1em;
        width: 1em;
        color: $dark-gray;
        cursor: pointer;
      }
      .input-field{
        padding: 1em 1em 1em 5em;
        box-shadow: $box-shadow;
        border-radius: 5px;
        border: none;
        font-size: 16px;
        width: calc(100%-5em);
        color: $dark-gray;
        font-family: inherit;
          &:focus{
            outline: none;
          }
      }
      .result-container{
        position: absolute;
        background-color: $white;
        border-radius: 5px;
        box-shadow: $box-shadow;
        width: 400px;
        z-index: 5;
        margin-top: 1em;
          .no-results{
            padding: 1em;
              p{
                margin: 0;
              }
          }
          .result{
            padding: 1em;
            display: flex;
            justify-content: space-between;
              .result-info{
                width: 50%;
                display: flex;
                flex-direction: column;
                justify-content: space-between;
                  p:first-child{
                    margin: 0;
                    font-size: 1.1em;
                    padding-bottom: .2em;
                    border-bottom: 1px solid $dark-gray;
                  }
                  p{
                    margin: 0;
                  }
              }
              .result-img{
                width: 40%;
              }
            &:hover{
              cursor: pointer;
              background-color: $white;
            }
          }
      }
    }
    .select{
      position: relative;
      padding: 1.5em 3em;
      appearance: none;
      border: none;
      padding: 1em 2em;
      font-family: inherit;
      outline: none;
      border: 1px solid $very_light_gray;
      box-shadow: $box-shadow;
      cursor: pointer;
      border-radius: 5px;
      margin-top: 2em;
      z-index: 1;
        &::after{
          content: "\f004";
          font-family: "FontAwesome";
        }
      @include breakpoint-up(medium){
        margin: 0;
      }
    }
    
  }
    .countries{
      padding: 2em;
      display: flex;
      flex-direction: column;
      align-items: center;
      @include breakpoint-up(medium){
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: space-between;
      }
      .country{
        border-radius: 10px;
        background-color: $white;
        font-size: 14px;
        margin-bottom: 3em;
        box-shadow: $box-shadow;
        max-width: 300px;
        &:hover{
          cursor: pointer;
        }
        @include breakpoint-up(medium){
          width: 40%;
          max-width: 230px;
          margin: 1em;
        }
        img{
          width: 100%;
          border-radius: 10px 10px 0 0;
        }
        .country-info{
          padding: 1em 1.5em;
            p:first-child{
              font-size: 1.5em;
            }
        }
      }
    }
    
}
</style>
