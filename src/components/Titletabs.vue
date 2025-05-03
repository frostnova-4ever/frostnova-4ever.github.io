<!-- 标签栏logo及其名字 -->

<template>
    <!-- 此组件没有具体的 DOM 结构 -->
    <div></div>
  </template>
  
  <script setup>
  import { onMounted } from 'vue';
  
  // 在组件挂载后设置标签页图标和标题
  onMounted(() => {
    // 设置标签页标题
    document.title = '你的页面标题';
  
    // 设置标签页图标
    const link = document.createElement('link');
    link.rel = 'icon';
    // 请将 'your-logo.ico' 替换为你实际的图标文件路径
    link.href = new URL('./assets/your-logo.ico', import.meta.url).href;
    const existingIcon = document.querySelector('link[rel="icon"]');
    if (existingIcon) {
      existingIcon.parentNode.replaceChild(link, existingIcon);
    } else {
      document.head.appendChild(link);
    }
  });
  </script>
  <style scoped>
  </style>