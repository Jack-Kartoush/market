<template>
  <section class="buyer-section p-3 border-bottom">
    <div
      class="d-flex justify-content-around align-items-center text-white fs-5"
    >
      <div class="d-flex flex-column align-items-start">
        <p>Profit</p>
        <p>Tax</p>
        <p>Price celing (Pc)</p>
        <p>Price floor (Pf)</p>
      </div>
      <div class="d-flex flex-column align-items-end">
        <p>100 points</p>
        <p>20%</p>
        <p>-</p>
        <p>-</p>
      </div>
    </div>
    <div class="d-flex align-items-end justify-content-end">
      <button class="btn btn-primary add" @click="emit('setUserTo', 'seller')">
        trad
      </button>
    </div>
  </section>
  <section class="buyer-body">
    <div class="d-flex justify-content-around">
      <div class="ask text-white p-3 text-center">
        <span>tesla 1 stock</span>
        <p>Ask: 100$</p>
      </div>
      <div class="highest_bid text-white p-3 text-center">
        <span>Highest Bid</span>
        <p>300$</p>
      </div>
    </div>
    <div class="navbar d-block">
      <div class="menu d-flex justify-content-around p-4">
        <div class="main-nav">
          <button class="btn btn-primary" @click="tab = 'bid'">bid</button>
          <button class="btn btn-primary" @click="tab = 'History'">
            History
          </button>
        </div>
        <button class="btn btn-outline-danger deleteBtn" @click="deleteAllItem()">
          Delete All
        </button>
      </div>
      <div class="bids-input d-flex justify-content-center">
        <input
          v-show="tab === 'bid'"
          type="number"
          placeholder="add your bid here"
          class="p-2"
          v-model="userBid"
        />
        <button
          v-show="tab === 'bid'"
          class="btn btn-primary"
          @click="Addbid()"
        >
          Add
        </button>
      </div>
    </div>
  </section>
  <section class="bid_section" v-show="tab === 'bid'">
    <div
      v-for=" (bid,index) in bids"
      class="d-flex justify-content-around align-items-center text-center p-3 bg-secondary text-white border-bottom"
    >
      <!-- <span>{{ bid.bidID }}</span> -->
      <span>{{ bid.userBid }} $</span>
      <span>{{ bid.currentDate }}</span>
      <button class="btn btn-primary" @click="deleteBidItem(index)"><i class="fa-solid fa-trash-can"></i></button>
    
    </div>
  </section>
  <section v-show="tab === 'History'">
    <h1 class="text-center p-3">History</h1>
    <div
      v-for=" (history,index) in historys"
      class="d-flex justify-content-around align-items-center text-center p-3 bg-secondary text-white border-bottom"
    >
      <!-- <span>{{ history.bidID }}</span> -->
      <span>{{ history.userBid }} $</span>
      <span>{{ history.currentDate }}</span>
      <button class="btn btn-primary" @click="deleteHistoryItem(index)"><i class="fa-solid fa-trash-can"></i></button>
      <button class="btn btn-primary" title="return it to the bid records"><i class="fa-solid fa-rotate-left"></i></button>
    </div>
  </section>
</template>
<script setup>
import { ref, onMounted, defineProps, toRefs, defineEmits } from "vue";
const props = defineProps(["showUser"]);
const emit = defineEmits(["seller", "setUserTo"]);
const { showUser } = toRefs(props);
const bids = ref([]);
const historys = ref([]);
const userBid = ref();
const bidID = ref(1);
const tab = ref("bid");
const currentDate = new Date().toISOString().slice(0, 10);
onMounted(() => {
  bids.value = JSON.parse(localStorage.getItem("bids") || "[]");
  historys.value = JSON.parse(localStorage.getItem("historys") || "[]");
});

function Addbid() {
  bids.value.push({
    userBid: userBid.value,
    bidID: bidID.value,
    currentDate: currentDate,
  });
  localStorage.setItem("bids", JSON.stringify(bids.value));


  historys.value.push({
    userBid: userBid.value,
    bidID: bidID.value,
    currentDate: currentDate,
  });
  localStorage.setItem("historys", JSON.stringify(historys.value));
  bidID.value++;
  userBid.value = null;
}

function deleteAllItem() {
  localStorage.removeItem("bids");
  localStorage.removeItem("historys");
  location.reload();
}
function deleteBidItem(index){
  bids.value.splice(index,1)
  localStorage.setItem("bids", JSON.stringify(bids.value));

}
function deleteHistoryItem(index){
  historys.value.splice(index,1)
  localStorage.setItem("historys", JSON.stringify(historys.value));
}
</script>
