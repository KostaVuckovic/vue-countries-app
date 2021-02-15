<template>
  <section class="container">
    <button class="btn-back" @click="$router.push({name: 'Home'})"><i class="fas fa-arrow-left"></i> Back</button>
    <div class="main">
      <img :src="countryInfo.flag" alt="flag">
        <div class="country-text">
          <p class="bolder name">{{countryInfo.name}}</p>
            <div class="country-info">
              <p><span class="bold">Native name: </span>{{countryInfo.nativeName}}</p>
              <p><span class="bold">Population: </span>{{countryInfo.population}}</p>
              <p><span class="bold">Region: </span>{{countryInfo.region}}</p>
              <p><span class="bold">Sub Region: </span>{{countryInfo.subregion}}</p>
              <p><span class="bold">Capital: </span>{{countryInfo.capital}}</p>
            </div>
            <div class="country-info-2">
              <p><span class="bold">Top Level Domain: </span><span v-for="(domain, index) in countryInfo.topLevelDomain" :key="index">
                {{comma(domain,index,countryInfo.topLevelDomain.length)}}
                </span></p>
              <p><span class="bold">Currencies: </span><span v-for="(curr, index) in countryInfo.currencies" :key="index">
                {{comma(curr.name,index,countryInfo.currencies.length)}}
                </span></p> 
              <p><span class="bold">Languages: </span><span v-for="(language, index) in countryInfo.languages" :key="index">{{comma(language.name,index,countryInfo.languages.length)}}</span></p>
            </div>
          <div class="borders" v-if="countryInfo.borders.length > 0">
            <p class="bold">Border countries: </p>
            <div class="border-countries">
              <div v-for="(border, index) in countryInfo.borders" :key="index" class="b-country" @click="borderCountryInfo(border)">{{border}}</div>
            </div>
          </div>
        </div>
    </div>
  </section>
  
</template>

<script>
import axios from 'axios'

export default {
name: 'CountryDetails',
props: ['country'],
data(){
  return{
    countryInfo: []
  }
},
watch: {
  '$route.params.country': function(country){
    this.getCountryInfo(country)
  }
},
mounted(){
  this.getCountryInfo(this.country)
},
methods: {
  getCountryInfo(country){
    axios.get(`https://restcountries.eu/rest/v2/name/${country}?fullText=true`)
      .then(response => {
        this.countryInfo = response.data[0]
      })
  },
  borderCountryInfo(border){
    axios.get(`https://restcountries.eu/rest/v2/alpha/${border}`)
      .then(response => {
        this.$router.push({path: `/${response.data.name}`})
      })
  },
  comma(item, index, length){
    if(length == 1 || index == length-1){
      return item
    }else if(length > 1 && index < length-1){
      return item + ', '
    }
  }
}
}
</script>

<style lang="scss" scoped>
@import "../sass/globals";
@import "../sass/mixins";

.container{
  background-color: $very-light-gray;
  padding: 1em 2em;
  max-width: 1350px;
  margin: 0 auto;
  @include breakpoint-up(large){
    padding: 1em 4em;
  }
    .btn-back{
      padding: .5em 1.5em;
      margin: 1.5em 0 2em 0;
      background-color: $white;
      border: none;
      border-radius: 5px;
      box-shadow: $box-shadow;
        &:hover{
          cursor: pointer;
        }
    }
    .main{
      width: 100%;
      max-width: 500px;
      margin: auto;
      @include breakpoint-up(large){
        display: flex;
        justify-content: space-between;
        align-items: center;
        max-width: 1350px;
        margin: 0;
      }
      img{
        width: 100%;
        @include breakpoint-up(large){
          width: 45%;
          max-width: 550px;
        }
      }
      .country-text{
        @include breakpoint-up(large){
          width: 45%;
          display: grid;
          grid-template-areas: "name name"
                               "left right"
                               "borders borders"
        }
        .name{
          font-size: 1.5rem;
          margin: 1em 0;
          @include breakpoint-up(large){
            grid-area: name;
          }
        }
        .country-info{
          @include breakpoint-up(large){
          grid-area: left;
          
        }
        }
        .country-info-2{
          padding-top: 1em;
          @include breakpoint-up(large){
            padding: 0;
            grid-area: right
          }
        }
        
        .borders{
          padding-top: 1em;
          @include breakpoint-up(large){
            grid-area: borders;
            display: flex;
            justify-content: space-between;
            // align-items: center;
          }
          p{
            font-size: 1.2rem;
            @include breakpoint-up(large){
              margin: 0 0 1em 0;
              font-size: 1em;
              margin-right: 4em;
            }
          }
          .border-countries{
            display: flex;
            flex-wrap: wrap;
            .b-country{
              padding: .5em 1.5em;
              width: 50px;
              box-shadow: $box-shadow;
              border-radius: 5px;
              margin: 0 .5em 1em 0;
              text-align: center;
                &:hover{
                  cursor: pointer;
                }
              @include breakpoint-up(large){
                width: 25px;
                font-size: 12px;
              }
            }
          }
        }
      }
      
    }
}
</style>