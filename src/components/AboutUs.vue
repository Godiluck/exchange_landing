<template>
  <div class="wrapper">
    <button v-on:click="appearInfo()" class="about_us">О нас</button>
    <div id="about" class="about_info">
      <div class="logos">
        <img src="../assets/logo1.png" alt="logo" class="logo">
        <img src="../assets/logo2.png" alt="logo" class="logo">
        <img src="../assets/logo3.png" alt="logo" class="logo">
        <img src="../assets/logo4.png" alt="logo" class="logo">
        <img src="../assets/logo5.png" alt="logo" class="logo">
      </div>
      <div class="about_text">
        <p>Мы самая известная и крупнейшая на Hydra команда обменников, в которые входили:</p>
        <ul class="exchange_list">
          <li>Моментальный Обмен 24-7</li>
          <li>Сбербанк Онлайн</li>
          <li>БИТКОИН БАНК</li>
          <li>Тинькофф Банк 24-7</li>
          <li>Круглосуточная поддержка 24-7</li>
        </ul>
        <p>Это самые известные из них, с которыми вы наверняка знакомы, а также еще свыше 15 других.</p>
        <p>Наша команда не изменилась, наш сервис - тоже, и это главное. Моментальный обмен и круглосуточная поддержка, которая всегда поможет - это то, почему выбирали именно нас.</p>
        <p>Здесь, работая не внутри площадки, у нас есть возможность убрать комиссии ресурса, а это значит, что обмен для вас станет еще выгоднее, без потери качества.</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AboutUs",
  data() {
    return {
      moduleState: 'closed',
    }
  },
  methods: {
  appearInfo() {
    document.getElementById('about').classList.toggle('open')
  }
}
}
</script>

<style scoped>
* {
  color: white;
}

.wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  width: 100%;
  cursor: default;
}

.about_us {
  margin-top: 20px;
  cursor: pointer;
  width: 130px;
  height: 50px;
  font-size: 25px;
  color: white;
  border: 1px solid white;
  border-radius: 50px;
  background: none;
  transition: .3s;
}

.about_us:hover {
  transform: scale(1.2);
  transition: .3s;
  box-shadow: 5px 5px 10px black;
}

.about_info {
  width: 100%;
  height: 0;
  opacity: 0;
  z-index: -1;
}

.open {
  height: auto;
  opacity: 1;
  z-index: initial;
}

.logos {
  margin: 30px 0 30px 0;
  width: 100%;
  display: flex;
  justify-content: space-evenly;
}

.logo {
  width: 50px;
  height: 50px;
}

.about_text {
  margin-left: 50px;
  display: flex;
  flex-direction: column;
  align-items: start;
}

p {
  margin: 5px 0 5px 0;
  text-align: start;
}

.exchange_list {
  list-style-type: circle;
  list-style-position: inside;
  margin: 20px 0 25px 0;
}

.exchange_list > li {
  text-align: start;
  margin-top: 5px;
}
</style>