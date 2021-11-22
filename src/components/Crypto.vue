<template>
	<div class="container">
		<input
        type="text"
        class="form-control text-light bg-dark rounded-0 border-0 my-4"
        placeholder="Search"
        v-model="textSearch"
        @keyup="searchCoin()"
        autofocus
      />

		<table class="table table-dark">
		  <thead>
		    <tr>
		      <th scope="col">#</th>
		      <th scope="col">Image</th>
		      <th scope="col">Price</th>
		      <th scope="col">Price Change</th>
		      <th scope="col">24h Volume</th>
		    </tr>
		  </thead>
		  <tbody>
		    <tr v-for="(item, index) in filteredCoins" :key="item.id">
		      <td scope="row">{{index + 1}}</td>
		      <th>
		      	<img :src="item.image" :alt="item.name" style="width: 2rem" class="me-2" >
		      	<span>{{ item.name }}</span>
		      	<span class="ms-2 text-muted text-uppercase">
                {{ item.symbol }}
            </span>
		      </th>
		      <td> $ {{ item.current_price.toLocaleString() }}</td>
		      <td :class="item.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger'"> {{ item.price_change_percentage_24h}}</td>
		       <td>{{ item.total_volume.toLocaleString() }}</td>
		    </tr>
		  </tbody>
		</table>
	</div>
	
</template>

<script>
export default {
  name: 'Crypto',
  props: {
  	datos: Array
  },
  data(){
  	 return{
  	 	 filteredCoins: [],
  	 	 textSearch: "",
  	 }
  },
   async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    this.filteredCoins = data;
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.datos.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
  
}
</script>

<style lang="css">
	body{
		background: #141414;
		color: #fff
	}
</style>