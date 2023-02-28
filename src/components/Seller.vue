<template>
  <section class="seller_section p-3 border-bottom">
    <form @submit.prevent="">
      <div class="d-flex flex-wrap align-items-start justify-content-center">
        <div class="d-block mt-3">
          <label for="name" class="text-white p-2">Company name(stock name)?</label>
          <input
            type="text"
            id="name"
            placeholder="Tesla"
            class="user-input"
            v-model="name"
            maxlength="10"
            autofocus
          />
        </div>
        <div class="d-flex flex-wrap mt-3">
          <label for="price" class="text-white p-2"> Stock lowest price?</label>
          <input
            type="number"
            id="price"
            placeholder="100$"
            min="1"
            class="user-input"
            v-model="price"
          />
        </div>
        <div class="d-block ms-1 mt-3">
          <label for="number" class="text-white">Stock Number? </label>
          <input
            type="number"
            placeholder="1"
            id="number"
            class="user-input"
            min="1"
            v-model="stockNumber"
          />
        </div>
      </div>
      <div class="d-flex align-items-end justify-content-end p-3">
        <button class="btn btn-primary mt-3 add " type="submit" @click="addSell">
          Add
        </button>
      </div>
    </form>
  </section>
  <section>
    <div class="list_container">
      <div
        v-for="(sell, index) in Sells"
        class="d-flex justify-content-evenly align-items-center p-3 list"
      >
        <span>{{ sell.name }}</span>
        <span>{{ sell.price }} $</span>
        <span>{{ sell.stockNumber }}</span>
        <div class="nav-menu">
        <button class="btn btn-outline-danger" @click="deleteSellsItems(index)">
          <i class="fa-solid fa-trash-can"></i>
        </button>
        <button class="btn btn-primary ms-3 item" type="submit" @click="setUserAndStock(sell.name)">
          Bid
        </button>
        </div>
      </div>
    </div>
  </section>
</template>
<script setup>
import { ref, onMounted, defineProps, toRefs, defineEmits, computed } from "vue";
const props = defineProps(["showUser", "Stock"]);
const { showUser, Stock } = toRefs(props);
const emit = defineEmits(["buyer", "setUserTo"]);
const Sells = ref([]);
let id = 0;
const name = ref("");
const price = ref("");
const stockNumber = ref();
onMounted(() => {
  Sells.value = JSON.parse(localStorage.getItem("Sells") || "[]");
  
});

function addSell() {
  // if(!name.value || !price.value || !stockNumber.value){
  //   alert("Ther is a on filled fild ")
  //   return;
  // }
  Sells.value.push({
    id: id,
    name: name.value,
    price: price.value,
    stockNumber: stockNumber.value,
  });
  // Stock.value.push({
  //   id: id,
  //   name: name.value,
  // });

  localStorage.setItem("Sells", JSON.stringify(Sells.value));
  // localStorage.setItem("Stock", JSON.stringify(Stock.value));
  name.value = null;
  price.value = null;
  stockNumber.value = null;

  id++;
}

function deleteSellsItems(index) {
  Sells.value.splice(index, 1);
  localStorage.setItem("Sells", JSON.stringify(Sells.value));
}

function setUserAndStock(stockName) {
  emit('setUserTo', 'buyer');
  emit('setStock', stockName);
}

</script>
