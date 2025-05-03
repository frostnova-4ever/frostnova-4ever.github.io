<template>
  <div class="index-toggle-btn">
    <btn text1="关闭索引" text0="打开索引"></btn>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import btn from './button/button.vue';
const isIndexVisible = ref(false);
const indexItems = ['A', 'B', 'C', 'D', 'E'];

const toggleIndex = () => {
  isIndexVisible.value =!isIndexVisible.value;
};

const handleIndexClick = (item) => {
  console.log(`你点击了索引项: ${item}`);
  isIndexVisible.value = false;
};
</script>

<style scoped>
/* 按钮样式 */
.index-toggle-btn {
  cursor: pointer;
  white-space: nowrap;
  color: black;
  text-decoration: none;
  position: relative;
  transition: all 0.3s ease;
}


/* 弹出框样式 */
.index-popup {
  position: absolute;
  background-color: white;
  border: 1px solid #ccc;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 10px;
  border-radius: 5px;
  z-index: 1;
}
.index-popup ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}
/* 列表项样式 */
.index-item {
  cursor: pointer;
  padding: 5px 10px;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}
.index-item:hover {
  background-color: #f0f0f0;
}
</style>