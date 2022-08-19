<template>
  <div class="wrapper">
    <div class="wrapper_inner">
      <div class="header">
        <h2>Информация по вашей заявке</h2>
        <p class="order_precautions">Внимание! Не обновляйте страницу, в противном случае вы не сможете отслеживать статус заявки</p>
        <p class="order_req">Переведите {{ this.dispatchSum }} RUB на реквизиты {{ requisites }}. После смены статуса на 'выплачена', сумма
          поступит вам в
          течение 5-10 минут на кошелек. Пожалуйста, подождите</p>
      </div>
      <div class="order_info">
        <ul class="order_key">
          <li>ID заявки</li>
          <li>Реквизиты для оплаты</li>
          <li>Сумма для отправки</li>
          <li>Сумма для получения</li>
          <li>Таймер оплаты</li>
          <li>Статус заявки</li>
        </ul>
        <ul class="order_value">
          <li>{{ orderId }}</li>
          <li>{{ requisites }}</li>
          <li>{{ dispatchSum }}</li>
          <li>{{ receivingSum }} {{ valute }}</li>
          <li id="timer">0</li>
          <li>{{ orderStatus }}</li>
        </ul>
      </div>
      <div @click="confirmPayment" id="confirm" class="confirm_payment">Я оплатил</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ModalWindow",
  props: {
    orderData: Object,
  },
  data() {
    return {
      data: this.orderData,
      dispatchSum: 0,
      receivingSum: 0,
      requisites: '0',
      orderId: 0,
      orderStatus: '0',
      createdAt: '0',
      valute: '0',
    }
  },
  methods: {
    fillGaps() {
      this.dispatchSum = this.orderData.give_amount
      this.receivingSum = this.orderData.get_amount
      this.requisites = this.data.exc_wallet
      this.orderId = this.data.orderid
      this.orderStatus = this.data.status
      this.createdAt = this.data.data_created
      this.valute = this.data.get_valute
      this.timerFunc()
    },
    timerFunc() {
      let minutes = 35;
      let seconds = 0;
      const t = setInterval(function () {
        if (seconds > 0) {
          seconds -= 1
          document.getElementById('timer').innerHTML = `${minutes} : ${seconds}`
        } else if (minutes > 0 && seconds === 0) {
          minutes -= 1
          seconds = 59
          document.getElementById('timer').innerHTML = `${minutes} : ${seconds}`
        } else {
          clearInterval(t)
          document.getElementById('timer').innerHTML = `00 : 00`
          this.orderStatus = 'Время оплаты вышло'
        }
      }, 1000)
    },
    getStatus() {
      const body = {
        action: 'check_status_order',
        orderid: this.orderId.toString(),
      }
      fetch("http://46.188.55.14:53344/excbot",
          {
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json',
            },
            method: 'POST',
            body: JSON.stringify(body)
          })
          .then((response) => response.json())
          .then((data) => {
            this.orderStatus = data.status
          })
    },
    confirmPayment() {
      const body = {
        action: "accept_order",
        orderid: this.orderId,
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
            this.orderStatus = data.status
            setInterval(this.getStatus, 6000)
          })
      this.getStatus()
      document.getElementById('confirm').style.display = 'none'
    }
  },
  watch: {
    orderData(newVal) {
      this.data = newVal
      this.fillGaps()
    }
  },
  created() {
    this.fillGaps()
  }
}
</script>

<style scoped>
* {
  color: white;
}

.wrapper {
  top: 0;
  margin: 100px 80px;
  left: 0;
  background: none;
  border-radius: 40px;
  border: 1px solid white;
  -webkit-box-shadow: 9px 6px 14px 2px rgba(0, 0, 0, 0.26);
  -moz-box-shadow: 9px 6px 14px 2px rgba(0, 0, 0, 0.26);
  box-shadow: 9px 6px 14px 2px rgba(0, 0, 0, 0.26);
}

.wrapper_inner {
  display: flex;
  flex-direction: column;
  border-radius: 40px;
  padding: 20px 30px;
}

.header {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

h2 {
  margin-top: 20px;
}

.order_precautions {
  margin-top: 30px;
  border-bottom: 1px solid white;
}

.order_req {
  margin-top: 30px;
  text-align: initial;
}

.order_info {
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin-top: 70px;
}

.order_key, .order_value {
  width: 50%;
}

ul {
  list-style-type: none;
  text-align: initial;
}

ul > li {
  margin-bottom: 10px;
  font-size: 25px;
  padding: 0 0 10px 30px;
  border-bottom: 2px solid white;
}

ul > li:first-child {
  padding-top: 10px;
}

ul > li:last-child {
  margin-bottom: 0;
}

.confirm_payment {
  border-radius: 20px;
  margin-top: 30px;
  width: 20%;
  cursor: pointer;
  line-height: 40px;
  align-self: center;
  height: 40px;
  background: none;
  color: white;
  border: 1px solid white;
  transition: .3s;
}

.confirm_payment:hover {
  transform: scale(1.2);
  transition: .3s;
  box-shadow: 5px 5px 10px #131674;
}
</style>