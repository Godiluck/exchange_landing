<template>
  <div class="wrapper">
    <div class="modules">
      <div class="module left_module">
        <div class="module_inner">
          <h3>Отдаю</h3>
          <div class="payment_method">
            <img class="payment_img" :src="leftWindow.img" alt="payment img">
            <div v-if="this.leftWindow.name === 'Card'" class="banks_list">
              <select @change="convertLR" id="bank">
                <option v-for="currency in this.rubCurrencies" :key="currency" :value="currency">{{ currency }}</option>
              </select>
            </div>
            <div v-else class="payment_name">{{ leftWindow.name }}</div>
          </div>
          <input @focus="defaultBorder('leftInp')" @input="convertLR" id="leftInp" type="number" :step="leftWindow.step"
                 :min=leftWindow.min
                 :max=leftWindow.max
                 :placeholder=leftWindow.placeholder>
        </div>
      </div>
      <div class="module right_module">
        <div class="module_inner">
          <h3>Получаю</h3>
          <div class="payment_method">
            <img class="payment_img" :src="rightWindow.img" alt="payment img">
            <div v-if="rightWindow.name === 'Card'" class="banks_list">
              <select @change="convertRL" id="bank">
                <option v-for="currency in this.rubCurrencies" :key="currency" :value="currency">{{ currency }}</option>
              </select>
            </div>
            <div v-else class="payment_name">{{ rightWindow.name }}</div>
          </div>
          <input id="rightInp" @input="convertRL" type="number" :step="rightWindow.step" :min=rightWindow.min
                 :max=rightWindow.max
                 :placeholder=rightWindow.placeholder>
          <p class="commission">*С учетом допкомиcсии сервиса</p>
          <div class="address_wrapper">
            <input @focus="defaultBorder('address')" class="address" id="address" @input="rightWindow.func"
                   :type="rightWindow.addressType"
                   :maxlength="rightWindow.length"
                   :placeholder="rightWindow.address">
          </div>
        </div>
      </div>
      <div class="swap_wrapper">
        <img @click="swap" class="swap" src="../assets/swap.svg" alt="swap button">
      </div>
    </div>
    <div class="confirm_wrapper">
      <div id="confirm" @click="leftWindow.confirm" class="confirm_btn">Обменять</div>
    </div>
  </div>
</template>

<script>
import btc from '../assets/btc.png'
import card from '../assets/card.png'

export default {
  name: "ExchangeModule",
  data() {
    return {
      BTC: {
        inpType: 'number',
        img: btc,
        name: 'Bitcoin',
        placeholder: 'Введите сумму от 0.000001 до 1 BTC',
        length: 36,
        step: 0.00000001,
        min: 0.00000100,
        max: 1.00000000,
        address: 'Адрес кошелька',
        addressType: 'text',
        func: null,
        confirm: this.emitBTCToRub
      },
      card: {
        inpType: 'tel',
        img: card,
        name: 'Card',
        placeholder: 'Введите сумму от 500 до 50 000 RUB',
        length: 19,
        step: 1,
        min: 500.00000000,
        max: 50000.00000000,
        address: 'Номер карты',
        addressType: 'tel',
        func: this.spaces,
        confirm: this.emitRubToBTC
      },
      leftWindow: {},
      rightWindow: {},
      modal: false,
      currencies: {},
      rubCurrencies: [],
      cryptoCurrency: '',
    }
  },
  methods: {
    defaultBorder(elem) {
      document.getElementById(elem).style.borderColor = 'white'
    },
    spaces() {
      document.getElementById('address').addEventListener('input', function (e) {
        e.target.value = e.target.value.replace(/[^\dA-Z]/g, '').replace(/(.{4})/g, '$1 ').trim();
      });
    },
    emitRubToBTC() {
      const input = document.getElementById('leftInp').value
      const address = document.getElementById('address').value.length
      if (input !== '' && address > 32 && input >= 500 && input <= 50000) {
        this.$emit('pushInfo', {success: true, header: 'Успех', body: 'Заявка была создана'})
        return this.createOrder()
      } else {
        if (input === '' || input === null || input < 500 || input > 50000) {
          document.getElementById('leftInp').style.borderColor = 'red'
        } else if (address === null || address === '' || address.length < 25) {
          document.getElementById('address').style.borderColor = 'red'
        }
        this.$emit('pushInfo', {success: false, header: 'Ошибка', body: `Введите корректные данные`})
      }
    },
    emitBTCToRub() {
      const input = document.getElementById('leftInp').value
      const address = document.getElementById('address').value
      if (input !== '' && input >= 0.000001 && input <= 1 && address.length === 19) {
        this.$emit('pushInfo', {success: true, header: 'Успех', body: 'Заявка была создана'})
        return this.createOrder()
      } else {
        if (input === '' || input === null || input > 1 || input < 0.000001) {
          document.getElementById('leftInp').style.borderColor = 'red'
        } else if (address === null || address === '' || address.length < 19) {
          document.getElementById('address').style.borderColor = 'red'
        }
        this.$emit('pushInfo', {success: false, header: 'Ошибка', body: `Введите корректные данные`})
      }
    },
    async createOrder() {
      if (this.leftWindow.name === 'Card') {
        const body = {
          action: "create_order",
          give_valute: document.getElementById('bank').value,
          get_valute: this.cryptoCurrency,
          give_amount: document.getElementById('leftInp').value,
          user_wallet: document.getElementById('address').value,
        }

        await fetch("http://46.188.55.14:53344/excbot",
            {
              headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
              },
              method: "POST",
              body: JSON.stringify(body)
            })
            .then((response) => response.json())
            .then((data) => {
              return this.$emit('data', data)
            })
      } else if (this.leftWindow.name === 'Bitcoin') {
        const body = {
          action: "create_order",
          give_valute: this.cryptoCurrency,
          get_valute: document.getElementById('bank').value,
          give_amount: document.getElementById('leftInp').value,
          user_wallet: document.getElementById('address').value,
        }

        await fetch("http://46.188.55.14:53344/excbot",
            {
              headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
              },
              method: "POST",
              body: JSON.stringify(body)
            })
            .then((response) => response.json())
            .then((data) => {
              return this.$emit('data', data)
            })
      }
      return this.$emit('modal', true)
    },
    convertLR() {
      if (this.leftWindow.name === 'Card') {
        const body = {
          action: "get_getamount",
          give_valute: document.getElementById('bank').value,
          get_valute: this.cryptoCurrency,
          give_amount: document.getElementById('leftInp').value,
        }

        fetch("http://46.188.55.14:53344/excbot",
            {
              headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
              },
              method: "POST",
              body: JSON.stringify(body)
            })
            .then((response) => response.json())
            .then((data) => {
              document.getElementById('rightInp').value = data
            })
      } else {
        const body = {
          action: "get_getamount",
          give_valute: this.cryptoCurrency,
          get_valute: document.getElementById('bank').value,
          give_amount: document.getElementById('leftInp').value,
        }

        fetch("http://46.188.55.14:53344/excbot",
            {
              headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
              },
              method: "POST",
              body: JSON.stringify(body)
            })
            .then((response) => response.json())
            .then((data) => {
              document.getElementById('rightInp').value = data
            })
      }
    },
    convertRL() {
      if (this.leftWindow.name === 'Card') {
        const body = {
          action: "get_getamount",
          give_valute: this.cryptoCurrency,
          get_valute: document.getElementById('bank').value,
          give_amount: document.getElementById('rightInp').value,
        }

        fetch("http://46.188.55.14:53344/excbot",
            {
              headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
              },
              method: "POST",
              body: JSON.stringify(body)
            })
            .then((response) => response.json())
            .then((data) => {
              document.getElementById('leftInp').value = data
            })
      } else {
        const body = {
          action: "get_getamount",
          give_valute: document.getElementById('bank').value,
          get_valute: this.cryptoCurrency,
          give_amount: document.getElementById('rightInp').value,
        }

        fetch("http://46.188.55.14:53344/excbot",
            {
              headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
              },
              method: "POST",
              body: JSON.stringify(body)
            })
            .then((response) => response.json())
            .then((data) => {
              document.getElementById('leftInp').value = data
            })
      }
    },
    swap() {
      document.getElementById('address').value = '';
      document.getElementById('rightInp').value = '';
      document.getElementById('leftInp').value = '';
      if (this.leftWindow === this.card) {
        this.leftWindow = this.BTC;
        this.rightWindow = this.card;
        document.getElementById('confirm').onclick = this.emitBTCToRub
      } else {
        this.leftWindow = this.card;
        this.rightWindow = this.BTC;
        document.getElementById('confirm').onclick = this.emitRubToBTC
      }
    },
    getCurrencies() {
      const body = {
        action: 'get_valutes'
      }

      fetch("http://46.188.55.14:53344/excbot",
          {
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
            method: "POST",
            body: JSON.stringify(body)
          })
          .then((response) => response.json())
          .then((data) => {
            this.currencies = data
            for (let currency in this.currencies) {
              if (this.currencies[currency] === 'RUB') {
                this.rubCurrencies.push(currency)
              } else {
                this.cryptoCurrency += currency
              }
            }
          })
    },
  },
  created() {
    this.getCurrencies()
    this.leftWindow = this.card;
    this.rightWindow = this.BTC;
  },
}
</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  width: 100%;
  margin-bottom: 70px;
  color: white;
}

.modules {
  position: relative;
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  width: 100%;
}

.module {
  background: none;
  width: 40%;
  height: 230px;
  -webkit-box-shadow: 9px 6px 14px 2px rgba(0, 0, 0, 0.26);
  -moz-box-shadow: 9px 6px 14px 2px rgba(0, 0, 0, 0.26);
  box-shadow: 9px 6px 14px 2px rgba(0, 0, 0, 0.26);
  border-radius: 40px;
  border: 1px solid white;
  padding: 20px 40px;
}

.module_inner {
  height: 100%;
}

.payment_method {
  display: flex;
  flex-direction: row;
  width: 70%;
  align-items: center;
}

h3 {
  margin-bottom: 20px;
}

.payment_img {
  width: 20px;
  height: fit-content;
}

.payment_name {
  font-size: 25px;
  margin-left: 20px;
}

.banks_list {
  cursor: pointer;
  margin-left: 20px;
  height: 29px;
  display: flex;
}

select {
  background: none;
  border: 1px solid white;
  text-align: center;
  border-radius: 10px;
  width: 170px;
  color: white;
  font-size: 18px;
  cursor: pointer;
}

input {
  background: none;
  margin-top: 20px;
  border: none;
  border-bottom: 1px solid white;
  width: 100%;
  color: white;
  font-size: 20px;
  font-weight: 600;
  padding-left: 15px;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input[type=number] {
  -moz-appearance: textfield;
}

::placeholder {
  color: white;
  opacity: 0.7;
}

:focus {
  outline: none;
}

.commission {
  text-align: left;
  font-size: 12px;
  margin-top: 10px;
}

.address {
  border: 1px solid white;
  border-radius: 7px;
  padding: 8px 12px;
}

.address_wrapper {
  height: 40%;
  display: flex;
  align-items: flex-end;
}

.swap_wrapper {
  margin-top: 25px;
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
}

.swap {
  height: 40px;
  padding: 10px;
  background: white;
  border-radius: 50%;
  cursor: pointer;
  transition: .3s;
}

.swap:hover {
  transform: scale(1.2);
  animation: rotation 1s;
}

.confirm_wrapper {
  margin-top: 40px;
  display: flex;
  justify-content: center;
  width: 100%;
}

.confirm_btn {
  background: none;
  color: white;
  padding: 9px 35px;
  font-size: 18px;
  font-weight: 800;
  border: 1px solid white;
  border-radius: 54px;
  cursor: pointer;
  transition: .3s;
}

.confirm_btn:hover {
  transform: scale(1.2);
  border: 1px solid rgba(0, 0, 0, 0);
  transition: .3s;
  box-shadow: 5px 5px 10px black;
  background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
  background-size: 400% 400%;
  animation: gradient 9s ease
}

@keyframes gradient {
  0% {
    background-position: 0 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0 50%;
  }
}

@keyframes rotation {
  from {
    transform: rotate(180deg) scale(1);
  }
  to {
    transform: rotate(0deg) scale(1.2);
  }
}
</style>