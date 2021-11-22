<template>
  <div class="home">
    <h1 class="text-center mt-5">Vue Crypto</h1>
    <Crypto :datos="arrayCrypto"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Crypto from './components/Crypto'

export default {
  name: 'Home',
  data(){
    return{
      arrayCrypto: []
    }
  },
  components: {
    Crypto,
  },
  methods: {
    async consumirApi(){
      try{
        const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')

        const data = await res.json()

        this.arrayCrypto = data

        console.log(data)
      }catch(error){
        console.log(error)
      }
    }
  },
  created(){
    this.consumirApi();
  }
}
</script>
