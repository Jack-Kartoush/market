<template>
  <header class="p-3">
    <h1
      class="text-white fw-bold text-center title m-0"
      v-if="showUser === 'seller'"
    >
      seller
    </h1>
    <div
      class="buyerheader d-flex justify-content-around text-white align-items-center"
      v-if="showUser === 'buyer'"
    >
      <h1 class="text-white fw-bold">buyer</h1>
      <vue-countdown
        :time="time"
        :interval="100"
        v-slot="{ days, hours, minutes, seconds }"
      >
        <p>market close in:</p>
        <span v-show="days >= 1">{{ days }}d </span>
        <span> {{ hours }}h {{ minutes }}m {{ seconds }}s</span>
      </vue-countdown>
    </div>
  </header>
  <main class="d-block">
    <Seller
      :Stock="Stock"
      :show-User="showUser"
      @setUserTo="setUserTo"
      @setStock="setStock"
      v-if="showUser === 'seller'"
    />
    <Buyer
      :Stock="Stock"
      :show-User="showUser"
      @setUserTo="setUserTo"
      v-else-if="showUser === 'buyer'"
    />
  </main>
</template>
<script setup>
import VueCountdown from "@chenfengyuan/vue-countdown";
import Seller from "./components/Seller.vue";
import Buyer from "./components/Buyer.vue";
import { ref, onMounted, watch, computed } from "vue";

const showUser = ref("seller");
// const Stock = ref([]);
const Stock = ref("BMW");
// const currentDate= new Date('February 14, 2023, 17:00:00');
const openMarket = new Date();
const closeMarketDate = new Date(openMarket.getFullYear() + 1, 0, 1);
const time = closeMarketDate - openMarket;
onMounted(() => {
  showUser.value = JSON.parse(localStorage.getItem("showUser") || ["seller"]);
  Stock.value = JSON.parse(localStorage.getItem("Stock") || ["BMW"]);
}),
  watch(showUser, (role) => {
    console.log(`role is ${role}`);
    localStorage.setItem("showUser", JSON.stringify(showUser.value));
  });

function setUserTo(role) {
  console.log("Setting role to", role);
  showUser.value = role;
  localStorage.setItem("showUser", JSON.stringify(role));
}

watch(Stock, (stockName) => {
  console.log(`stockName is ${stockName}`);
  localStorage.setItem("Stock", JSON.stringify(Stock.value));
});

function setStock(stockName) {
  console.log("Setting stockName to", stockName);
  Stock.value = stockName;
  localStorage.setItem("Stock", JSON.stringify(stockName));
}
</script>
