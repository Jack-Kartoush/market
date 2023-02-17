<template>
  <section class="seller_section p-3 border-bottom">
    <div class="form d-flex flex-wrap align-items-start justify-content-center ">
      <div class="d-block mt-3">
        <label for="name" class="text-white p-2">Company name(stock name)?</label>
        <input
          type="text"
          id="name"
          placeholder="Tesla"
          class="user-input"
          v-model="name"
        />
      </div>
      <div class="d-flex flex-wrap mt-3">
        <label for="price" class="text-white p-2"> Stock lowest price?</label>
        <input
          type="number"
          id="price"
          placeholder="100"
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
      
      <button class="btn btn-primary mt-3 add" type="submit" @click="addSell">Add</button>
    </div>
  </section>
  <section>
    <div
      v-for="sell in Sells"
      class="d-flex justify-content-around align-items-center text-center p-3 bg-secondary text-white border-bottom"
    >
      <span>{{ sell.name }}</span>
      <span>{{ sell.price }} $</span>
      <span>{{ sell.stockNumber }}</span>

      <button class="btn btn-primary" type="submit" @click="emit('setUserTo', 'buyer')">
        Bid
      </button>
    </div>
  </section>
</template>
<script setup>
import { ref, onMounted, defineProps, toRefs, defineEmits, computed } from "vue";
const props = defineProps(["showUser"]);
const { showUser } = toRefs(props);
const emit = defineEmits(["buyer", "setUserTo"]);
const Sells = ref([]);
const id = ref(1);
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
    // money_sign: money_sign.value,
    id: id.value,
    name: name.value,
    price: price.value,
    stockNumber: stockNumber.value,
  });
  
  localStorage.setItem("Sells", JSON.stringify(Sells.value));
  name.value = null;
  price.value = null;
  stockNumber.value = null;
  id.value++;
}
// const buyer= computed(() =>{
//   return showUser.value === 'buyer'
// })
</script>
