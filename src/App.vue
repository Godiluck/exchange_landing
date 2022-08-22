<template>
  <div class="main_wrapper">
    <PageHeader/>
    <PageTerms/>
    <div class="router">
      <div class="exchange" v-if="modal === false">
        <ExchangeModule
            @modal="toggleModal"
            @data="getOrder"
            @pushInfo="getOperationInfo"
        />
      </div>
      <div class="order" v-else>
        <ModalWindow
            :order-data="orderInfo"
        />
      </div>
    </div>
    <PageFooter/>
    <PushBox
        class="notification"
        :operation-info="operationInfo"
    />
  </div>
</template>

<script>
import PageHeader from './components/PageHeader'
import PageTerms from "@/components/PageTerms";
import ExchangeModule from "@/components/ExchangeModule";
import PageFooter from "@/components/PageFooter";
import ModalWindow from "@/components/ModalWindow";
import PushBox from "@/components/PushBox";

export default {
  name: 'App',
  data() {
    return {
      orderInfo: {},
      modal: false,
      operationInfo: {},
    }
  },
  components: {
    PushBox,
    ModalWindow,
    PageFooter,
    ExchangeModule,
    PageTerms,
    PageHeader
  },
  methods: {
    toggleModal(state) {
      this.modal = state
    },
    getOrder(order) {
      this.orderInfo = order
    },
    getOperationInfo(obj) {
      this.operationInfo = obj
    }
  },
}
</script>

<style>

* {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #131674;
}

.main_wrapper {
  position: relative;
  background-image: linear-gradient( 109.6deg,  rgba(48,207,208,1) 11.2%, rgba(51,8,103,1) 92.5% );
}

.notification {
  position: fixed;
  top: 20vh;
  right: 30px;
  z-index: +1;
}
</style>
