<template>
  <div>
    <Header/>
    <BacaArticle/>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue'
import BacaArticle from '@/components/BacaArticle.vue'

export default {
  name: 'home',
  components: {
    Header,
    BacaArticle
  },
  data(){
    return{

    }
  }
}
</script>

<style lang="css">
</style>
