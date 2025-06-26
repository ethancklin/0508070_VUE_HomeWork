<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const NetworkAddressName = ref('');
const Network = ref([]);
const key = 'auo-device';

onMounted(() => {
  const storage = localStorage.getItem(key);

  if (storage != null) {
    const result = JSON.parse(storage);
    Network.value = result;
  }
});

const AddNetworkAddress = () => {
  if (NetworkAddressName.value != '') {
    const NetworkAddressList = {
      id: crypto.randomUUID(),
      name: NetworkAddressName.value,
    };
    Network.value.unshift(NetworkAddressList);

    localStorage.setItem(key, JSON.stringify(Network.value));

    NetworkAddressName.value = '';
  }
};
const clearNetworkAddress = () => {
  Network.value = [];
  localStorage.removeItem(key);
};

const clearNetworkAddressSingle = (index) => {
  //console.log(Network.value[index].id)
  const dataStr = localStorage.getItem(key);
  let dataArr = [];
  if (dataStr) {
    dataArr = JSON.parse(dataStr);
  }
  //console.log(dataArr)
  let targetId = Network.value[index].id;
  // 過濾掉要刪除的資料
  Network.value.splice(index, 1);
  const newDataArr = dataArr.filter((item) => item.id !== targetId);

  // 將更新後的資料存回
  localStorage.setItem(key, JSON.stringify(newDataArr));
};
</script>

<template>
  <h1 class="text-5xl flex space-x-2">
    收藏網址⼩⼯具

    <button @click="clearNetworkAddress" class="btn btn-error btn-sl mx-3">
      刪除全部收藏
    </button>
  </h1>

  <input
    type="text"
    class="input"
    @keyup.enter="AddNetworkAddress"
    v-model.trim="NetworkAddressName"
  />
  <button
    @click="AddNetworkAddress"
    class="btn bg-purple-300 px-2 py-1 mx-8 my-5 rounded hover:bg-purple-400"
  >
    新增收藏
  </button>

  <hr class="my-2" />

  <ul>
    <li v-for="(item, index) in Network">
      <a :href="item.name" target="_blank">{{
        index + 1 + '-->' + item.name
      }}</a>
      <button
        @click="clearNetworkAddressSingle(index)"
        class="btn btn-warning btn-sl mx-2"
      >
        刪除
      </button>
    </li>
  </ul>
</template>
