<template>
  <nav class="navbar">
    <div class="container">
      <p class="heading bolder" @click="$router.push({name: 'Home'})">Where in the world?</p>
      <p class="theme-switcher bold"><i class="far fa-moon"></i> Dark Mode</p>
    </div>
    
  </nav>
</template>

<script>
export default {
name: 'Navigation'
}
</script>

<style lang="scss" scoped>
@import "../sass/globals";
@import "../sass/mixins";

.navbar{
  display: grid;
  grid-template-columns: 1em auto 1em;
  grid-template-rows: 2em auto 2em;
  border-bottom: 1px solid $dark-gray;
  box-shadow: $box-shadow;
  @include breakpoint-up(large){
    grid-template-columns: 4em auto 4em;
  }
  @include breakpoint-up(xlarge){
    grid-template-columns: auto 1350px auto;
  }
    .container{
      display: flex;
      justify-content: space-between;
      align-items: center;
      grid-column-start: 2;
      grid-row-start: 2;
      @include breakpoint-up(xlarge){
        max-width: 1350px;
      }
        .heading{
          margin: 0;
            &:hover{
              cursor: pointer;
            }
        }
        .theme-switcher{
          margin: 0;
        }
    }
    
}
</style>