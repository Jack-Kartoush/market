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
        <span v-show="days >= 1">{{ days }}d</span>
        <span>{{ hours }}h {{ minutes }}m {{ seconds }}s</span>
      </vue-countdown>
    </div>
  </header>
  <main class="d-block">
    <Seller
      :show-User="showUser"
      @setUserTo="setUserTo"
      v-if="showUser === 'seller'"
    />
    <Buyer
      :show-User="showUser"
      @setUserTo="setUserTo"
      v-else-if="showUser === 'buyer'"
    />
  </main>
</template>
<script setup>
import VueCountdown from '@chenfengyuan/vue-countdown';
import Seller from "./components/Seller.vue";
import Buyer from "./components/Buyer.vue";
import { ref, onMounted,watch } from "vue";

const showUser = ref("buyer");

// const currentDate= new Date('February 14, 2023, 17:00:00');
const openMarket = new Date();
const closeMarketDate =new Date(openMarket.getFullYear() + 1, 0, 1);
console.log("openMarket", openMarket)
console.log("closeDate", closeMarketDate)
const time = closeMarketDate - openMarket;

onMounted(() => {
  showUser.value = JSON.parse(localStorage.getItem("showUser") || '[buyer]')

}),
watch(showUser, (role) =>{
  console.log(`role is ${role}`)
  localStorage.setItem("showUser", JSON.stringify(showUser.value));

})
function setUserTo(role) {
  console.log("Setting role to", role);
  showUser.value = role;
}

</script>
