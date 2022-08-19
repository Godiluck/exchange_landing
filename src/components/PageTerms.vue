<template>
  <div class="wrapper">
    <div class="module">
      <h2>Моментальный обмен 24/7</h2>
      <ul>
        <li>Переводите ровно ту сумму, которую вы указали в заявке</li>
        <li>Время на оплату заявки - 35 мин</li>
      </ul>
      <h2>Остерегайтесь мошенников! Сохраните нашу ссылку:</h2>
      <ul>
        <li><a href="http://momentwpfug2jsfakwj74wp4zok5p2ub2secsw5kn4arj2nl5noxw5ad.onion/">Зеркало в Tor'e</a></li>
        <li><a href="https://moment-obmen247.io/">https://moment-obmen247.io</a></li>
      </ul>
    </div>
    <div class="logo">
      <img class="logo_atm" src="../assets/logo-atm.jpg" alt="logo-atm">
    </div>
  </div>
</template>

<script>
export default {
  name: "PageTerms"
}
</script>

<style scoped>
* {
  color: white;
  cursor: default;
}

.wrapper {
  margin: 30px 50px;
  display: flex;
  height: auto;
  flex-direction: row;
  justify-content: space-between;
}

.module {
  background: none;
  border-radius: 50px;
  text-align: start;
  display: flex;
  padding: 10px 20px;
  flex-direction: column;
  justify-content: space-evenly;
}

a {
  text-decoration: none;
  cursor: pointer;
}

h2 {
  margin-top: 10px;
}

ul {
  list-style-position: inside;
}

h2:first-child {
  margin-top: 0;
}

li:first-child {
  margin-top: 0;
}

li {
  margin-top: 10px;
}

.logo {
  height: 100%;
  display: flex;
}

.logo_atm {
  height: 300px;
  border-radius: 38% 62% 35% 65% / 42% 45% 55% 58% ;
  transition: .3s ease-out;
}

.logo_atm:hover {
  box-shadow: 5px 5px 10px black;
  transform: scale(1.2);
  transition: .3s ease-out;
}
</style>