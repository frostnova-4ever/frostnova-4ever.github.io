<template>
  <div class="search-container">
    <input
      v-model="searchText"
      type="text"
      placeholder="请输入搜索内容"
      class="search-input"
    />
    <button @click="handleSearch" class="search-button">
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="white" viewBox="0 0 16 16">
        <path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"/>
      </svg>
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const searchText = ref('');

const handleSearch = () => {
  if (searchText.value.trim()!== '') {
    console.log('执行搜索操作，搜索关键词为：', searchText.value);
    // 这里可以添加实际的搜索逻辑，比如调用后端接口
  }
};
</script>

<style scoped>
.search-container {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  border-radius: 20px;
  overflow: hidden;
  background-color: #fff;
  height: 40px;
}

.search-input {
  flex: 1;
  width: 100%;
  height: 100%;
  padding: 1px;
  border: none;
  outline: none;
}

.search-button {
  height: 100%;
  background-color: #007bff;
  border: none;
  padding: 10px 15px;
  cursor: pointer;
}
</style>
  