<!-- 主页右侧的时间组件 -->

<template>
    <div class="date-time-display">
      <h2 class="date-time-display__title">
        <svg class="date-time-display__icon" aria-hidden="true">
          <use xlink:href="#icon-clock"></use>
        </svg>
        当前日期与时间
      </h2>
      <p class="date-time-display__content">{{ formattedDateTime }}</p>
    </div>
  </template>
  
  <style scoped>
  .date-time-display {
    background-color:  rgba(255,255, 255, 0.5);
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 10px;
  }
  
  .date-time-display__title {
    display: flex;
    align-items: center;
    font-size: 20px;
    margin-bottom: 10px;
  }
  
  .date-time-display__icon {
    width: 20px;
    height: 20px;
    margin-right: 10px;
  }
  
  .date-time-display__content {
    font-size: 16px;
    color: #333;
  }
  </style>
  
  <script setup>
  import { ref, onMounted, onUnmounted } from 'vue';
  
  const formattedDateTime = ref('');
  let intervalId;
  
  const updateDateTime = () => {
    const now = new Date();
    const year = now.getFullYear();
    const month = String(now.getMonth() + 1).padStart(2, '0');
    const day = String(now.getDate()).padStart(2, '0');
    const hours = String(now.getHours()).padStart(2, '0');
    const minutes = String(now.getMinutes()).padStart(2, '0');
    const seconds = String(now.getSeconds()).padStart(2, '0');
  
    formattedDateTime.value = `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
  };
  
  onMounted(() => {
    updateDateTime();
    intervalId = setInterval(updateDateTime, 1000);
  });
  
  onUnmounted(() => {
    clearInterval(intervalId);
  });
  </script>
      