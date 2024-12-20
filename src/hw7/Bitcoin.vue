<template>
  <div class="wrapper">
    <h1>Bitcoin Price Calculator</h1>
    <p>Выберите валюту для расчета:</p>
    <select v-model="currency" @change="getBitcoinPrice">
      <option v-for="option in currencyOptions" :key="option" :value="option">
        {{ option }}
      </option>
    </select>

    <p>Введите количество биткоинов:</p>
    <input type="number" v-model="amount" placeholder="Количество BTC" />

    <button v-if="currency && amount" @click="calculatePrice">
      Рассчитать стоимость
    </button>
    <button disabled v-else>Выберите валюту и количество</button>

    <p class="error">{{ error }}</p>

    <div v-if="totalPrice !== null">
      <p>Стоимость {{ amount }} BTC в {{ currency }}: {{ totalPrice }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      currency: '',
      amount: 1,
      error: '',
      bitcoinPrice: null,
      totalPrice: null,
      currencyOptions: ['USD', 'EUR', 'GBP', 'RUB'],
    };
  },
  methods: {
    async getBitcoinPrice() {
      try {
        this.error = '';
        const response = await axios.get(
          `https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=${this.currency.toLowerCase()}`
        );
        this.bitcoinPrice = response.data.bitcoin[this.currency.toLowerCase()];
      } catch (error) {
        this.error = 'Ошибка при получении данных. Пожалуйста, попробуйте снова.';
        console.error(error);
      }
    },
    calculatePrice() {
      if (this.amount > 0 && this.bitcoinPrice) {
        this.totalPrice = (this.amount * this.bitcoinPrice).toFixed(2);
      } else {
        this.error = 'Введите корректное количество биткоинов.';
      }
    },
  },
};
</script>

<style scoped>
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #1f0f24;
  text-align: center;
  color: #fff;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button:disabled {
  background: #746027;
  cursor: not-allowed;
}

.wrapper button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.error {
  color: #d03939;
}
</style>


