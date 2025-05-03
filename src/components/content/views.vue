<!-- 累计观看数量 -->

<template>
    <div class="article-item">
      <div class="article-title">累计观看数量</div>
      <div class="article-date">{{ viewCount }}</div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  // 从本地存储中获取之前的观看数量，如果没有则初始化为0
  const viewCount = ref(parseInt(localStorage.getItem('viewCount')) || 0);
  
  onMounted(() => {
    // 每次组件挂载（页面加载或刷新）时，观看数量加1
    viewCount.value++;
    // 将更新后的观看数量存储到本地存储中
    localStorage.setItem('viewCount', viewCount.value.toString());
  });
  </script>
  
  <style scoped>
  .article-item {
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    padding: 20px;
    margin-bottom: 10px;
    background-color: #fff;
  }
  .article-title {
    font-size: 18px;
    color: #333;
    margin-bottom: 5px;
  }
  .article-date {
    font-size: 14px;
    color: #666;
  }
  </style>