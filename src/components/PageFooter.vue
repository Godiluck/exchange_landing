<template>
  <div class="wrapper">
    <div class="contact">
      <a href="https://t.me/exchangeflash_support"><img src="../assets/tg.png" alt="tg img"></a>
      <a href="https://t.me/exchangeflash_support">Чат поддержки</a>
    </div>
  </div>
</template>

<script>
export default {
  name: "PageFooter",
}
</script>

<style scoped>
.wrapper {
  width: 100%;
  padding: 30px 0;
  background: none;
  display: flex;
  justify-content: space-around;
  align-items: center;
  border-top: 1px solid white;
  border-radius: 40px 40px 0 0;
}

.contact {
  display: flex;
  flex-direction: row;
  align-items: center;
  position: relative;
}

a {
  color: white;
  margin-left: 45px;
  padding-bottom: 3px;
  cursor: pointer;
  box-sizing: border-box;
  border-bottom: 1px solid rgba(0, 0, 0, 0);
  text-decoration: none;
  transition: .3s ease-in;
}

a:hover {
  border-bottom: 1px solid white;
  transition: .3s ease-in;
}

img {
  position: absolute;
  cursor: pointer;
  transition: .2s ease-in;
  bottom: 1px;
}

img:hover {
  transform: scale(1.5);
  transition: .2s ease-in;
}

</style>