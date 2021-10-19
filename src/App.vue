<template>
  <div class="container">
    <h1>{{ title }}</h1>
    <!-- v-on:keyup="search" -->
    <input
      @keyup="search"
      v-model="textSearch"
      type="search"
      style="text-align:center"
      placeholder="Search.."
      class="form-control bg-dark text-light my-2"
    />

    <table class="table table-dark">
      <thead>
        <tr>
          <th>#</th>
          <th>Coin</th>
          <th>Symbol</th>
          <th>Price</th>
          <th>Price Change</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="coin in coinsCopy" :key="coin.id">
          <td class="text-muted">{{ coin.market_cap_rank }}</td>
          <td>
            <img width="35" :src="coin.image" :alt="coin.id" />
            {{ coin.name }}
          </td>
          <td class="text-uppercase text-muted">{{ coin.symbol }}</td>
          <td>$ {{ coin.current_price }}</td>
          <td
            :class="[
              coin.price_change_24h < 0 ? 'text-danger' : 'text-success',
            ]"
          >
            {{ coin.price_change_24h }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <!-- <ol>
    <li v-for="coin in coins" v-bind:key="coin.id">
      {{ coin.name }}
    </li>
  </ol> -->
</template>

<script>
export default {
  name: "App",
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    this.coins = await res.json();
    this.coinsCopy = this.coins;
    console.log(this.coins);
  },
  data() {
    return {
      textSearch: "",
      coinsCopy: [],
      coins: [],
      title: "Hola desde vue",
    };
  },
  methods: {
    search() {
      console.log("search");
      // e.preventDefault();
      let value = this.textSearch.toLowerCase();
      this.coinsCopy = this.coins.filter(
        ({ name, symbol }) =>
          name.toLowerCase().includes(value) ||
          symbol.toLowerCase().includes(value)
      );
    },
  },
};
</script>

<style></style>
