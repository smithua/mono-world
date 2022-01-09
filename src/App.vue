<template>
  <div class="exchange-rate__container">
    <strong>{{ parentMessage }}</strong>
    <ul class="exchange-rate__list">
      <li v-for="data in exchangeRateData" v-bind:key="data.id">
        <span>{{ data.currencyCodeA }}</span> - <span> {{data.rateBuy}} / {{data.rateSell}} </span>
      </li>
    </ul>
  </div>

  <footer class="footer">
    <div class="magic">
      <p>
        check out the
        <a class="links" href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
      </p>
      <h3>Ecosystem</h3>
      <ul class="some-links">
        <li><a class="links" href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
        <li><a class="links" href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
        <li><a class="links" href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
        <li><a class="links" href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
        <li><a class="links" href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
        <li><a class="links" href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
      </ul>
    </div>
  </footer>
</template>

<script>
import axios from "axios";

export default {
  beforeCreate() {
    let isExchangeRate = window.localStorage.getItem("isExchangeRate") === 'true';

    async function setExchangeRateData() {
      try {
        let response = await axios.get('https://api.monobank.ua/bank/currency');
        window.localStorage.setItem("exchangeRateField", JSON.stringify(response.data));
        window.localStorage.setItem("isExchangeRate", true);
      } catch (error) {
        console.error(error);
      }
    }

    if (isExchangeRate) return false;
    if (!isExchangeRate) setExchangeRateData();
  },
  created() {
    this.exchangeRateData[0].currencyCodeA = this.exchangeRateData[0].currencyCodeA === 840 ? "USD" : this.exchangeRateData[0].currencyCodeA;
    this.exchangeRateData[1].currencyCodeA = this.exchangeRateData[1].currencyCodeA === 978 ? "EUR" : this.exchangeRateData[1].currencyCodeA;
    this.exchangeRateData[2].currencyCodeA = this.exchangeRateData[2].currencyCodeA === 985 ? "PLN" : this.exchangeRateData[2].currencyCodeA;
  },
  data() {
    return {
      parentMessage: "Exchange rate",
      exchangeRateData:
        JSON.parse(window.localStorage.getItem("exchangeRateField")).filter(data => data.currencyCodeA == 840 || (data.currencyCodeA == 978 && data.currencyCodeB == 980) || data.currencyCodeA == 985)
    }
  }
}
</script>

<style>
*, *::after, *::before {
  box-sizing: border-box;
}
html, body {
  width: 100%;
  height: 100%;
}
body {
  margin: 0;
  background-color: #41b983;
}

html {
  font-family: Blanco,system-ui,-apple-system,BlinkMacSystemFont,Segoe UI,Roboto,Helvetica,Arial,sans-serif,Apple Color Emoji,Segoe UI Emoji,Segoe UI Symbol;
  font-size: 16px;
  font-weight: 400;
  line-height: 1.7;

  -webkit-text-size-adjust: 100%;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.app {
  text-align: center;
  color: #2c3e50;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.exchange-rate__container {
  max-width: 400px;
  margin: 0 auto;

  margin-top: 20vh;
  flex-grow: 1;

  color: #fff;
  font-weight: 300;
  font-size: 30px;
}

.exchange-rate__list {
  display: flex;
  flex-direction: column;
  text-align: left;
}

/* Other Stuff */

.magic {
  visibility: hidden;
}
.footer:hover .magic {
  visibility: visible;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
ul li {
  display: inline-block;
  margin: 0 10px;
}
.links,
.links:hover,
.links:visited {
  color: #fff;
}

.links:hover {
  text-decoration: none;
}
</style>
