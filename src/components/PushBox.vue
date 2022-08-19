<template>
  <div id="pushBox" class="push_box">
    <div id="pushHeader" class="push_header">{{ header }}</div>
    <div id="pushBody" class="push_body">{{ body }}</div>
  </div>
</template>

<script>
export default {
  name: "PushBox",
  props: {
    operationInfo: Object,
  },
  data() {
    return {
      success: false,
      header: '',
      body: '',
    }
  },
  methods: {
    dropModal() {
      const pushBox = document.getElementById('pushBox')
      const header = document.getElementById('pushHeader')
      const body = document.getElementById('pushBody')
      if (this.success === true) {
        header.style.background = '#131674'
        body.style.background = '#1F9BEB'
      } else {
        header.style.background = '#D51010'
        body.style.background = '#EF2C2C'
      }
      pushBox.classList.add('show')
      setTimeout(function () {
        pushBox.classList.remove('show')
      }, 4000)
    }
  },
  watch: {
    operationInfo(newVal) {
      this.success = newVal.success
      this.header = newVal.header
      this.body = newVal.body
      this.dropModal()
    }
  }
}
</script>

<style scoped>
* {
  color: white;
}

*:hover {
  cursor: default;
}

.push_box {
  visibility: hidden;
  display: flex;
  flex-direction: column;
  width: 270px;
  height: 130px;
  border-radius: 15px;
}

.push_box:hover {
  opacity: 1;
}

.push_header {
  border-radius: 15px 15px 0 0 ;
  width: 100%;
  height: 30%;
  background: #23a6d5;
  display: flex;
  justify-content: center;
  align-items: center;
}

.push_body {
  width: 100%;
  height: 70%;
  background: #131674;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 0 0 15px 15px;
}

.show {
  visibility: visible;
  animation: fadeIn .4s;
}

@keyframes fadeIn {
  from {opacity: 0}
  to {opacity: 1}
}
</style>