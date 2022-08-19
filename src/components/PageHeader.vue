<template>
  <div class="wrapper">
    <p class="exchange_currency">1 BTC = {{ this.btc }} RUB</p>
  </div>
</template>

<script>
export default {
  name: "PageHeader",
  data() {
    return {
      btc: 0,
    }
  },
  methods: {
    async getToken() {
      await fetch('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=rub')
      .then((response) => response.json())
      .then((data) => {
        this.btc = data.bitcoin.rub.toLocaleString()
      })
    }
  },
  created() {
    this.getToken()
  }
}
</script>

<style scoped>
.wrapper {
  width: 100%;
  height: 80px;
  background: none;
  border-bottom: 1px solid white;
  border-radius: 0 0 40px 40px;

  display: flex;
  justify-content: center;
  align-items: center;
}

.exchange_currency {
  color: white;
  font-size: 30px;
}
</style>