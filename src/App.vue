<template>
  <div class="container">
    <div class="row">
      <h1 class="text-center text-uppercase my-2">
        {{ titleApp }}
      </h1>

      <input
        type="text"
        class="form-control bg-dark text-light rounded-0 border-0 my-3"
        placeholder="Search coin"
        @keyup="searchCoin()"
        v-model="textSearch"
      />

      <table class="table table-dark table-hover table-striped">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">
              {{ title }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td class="text-muted">
              {{ index + 1 }}
            </td>

            <td>
              <img class="img-fluid me-2" :src="coin.image" />
              <span>
                {{ coin.name }}
              </span>
              <span class="ms-2 text-uppercase text-muted">
                {{ coin.symbol }}
              </span>
            </td>
            <td>$ {{ coin.current_price }}</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
                coin.change_percentage > 0 ? 'fa-arrow-up' : 'fa-arrow-down',
              ]"
            >
              {{ coin.price_change_percentage_24h }} %
            </td>
            <td>$ {{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titleApp: "Coin Market",
      titles: ["#", "Coin", "Price", "Price Change", "24h Volume"],
      textSearch: "",
    };
  },
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    console.log(data);
    this.coins = data;
    this.filteredCoins = data;
  },

  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter((coin) => {
        return (
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
        );
      });
    },
  },
};
</script>

<style>
td img {
  width: 2rem;
  object-fit: cover;
}
</style>
