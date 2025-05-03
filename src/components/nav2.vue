<template>
  <nav class="navigation-bar">
    <div class="logo-container">
      <img :src="logoSrc" alt="Logo" class="logo" />
    </div>
    <!-- 使用容器包裹导航链接和 GitHub 图标 -->
    <div class="nav-right-container">
      <ul class="nav-links">
        <li class="nav-item" v-for="(link, index) in navLinks" :key="index">
          <a :href="link.url">{{ link.text }}</a>
        </li>
      </ul>
      <a href="https://github.com" target="_blank" class="github-icon">
        <img src="https://th.bing.com/th/id/ODLS.b2099a11-ca12-45ce-bede-5df940e38a48?w=32&h=32&qlt=90&pcl=fffffa&o=6&pid=1.2" alt="GitHub Icon">
      </a>
    </div>
  </nav>
</template>

<script setup>
import { ref } from 'vue';

// 定义 logo 图片的链接
const logoSrc = ref(new URL('../assets/ep.png', import.meta.url).href);

// 定义导航链接数组
const navLinks = ref([
  { text: '首页', url: '/' },
  { text: '关于', url: '/about' },
  { text: '服务', url: '/services' },
  { text: '联系我们', url: '/contact' }
]);
</script>

<style scoped>
  .navigation-bar {
    /* 空间属性 */
    display: flex;
    position: fixed;
    /* 颜色 */
    transition: background-color 0.3s ease;
    background-color: rgba(0, 0, 0, 0.1);
    color: white;
    /* 位置 */
    justify-content: space-between;
    align-items: center;
    padding: 5px 20px;
    height: 5vh;
    top: 0;
    left: 0; 
    width: 98%;
    /* 层级 */
    z-index: 1000; 

}

.logo-container img{
  width: 30px;
  height: 30px;
  object-fit: cover;
}

.logo {
  display: flex;
  align-items: center;
}

.nav-right-container {
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

.nav-links {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
  justify-content: center;
}

.nav-item {
  margin-left: 15px;
  cursor: pointer;
  font-size: 20px; 
  white-space: nowrap; 
  opacity: 0.4;
}

.nav-item a {
  text-decoration: none;
  color: #ffffff;
}

.nav-item a:hover {
  text-decoration: underline;
}

.github-icon {
  margin-left: 15px; 
  display: flex;
  align-items: center;
}

.github-icon img {
  width: 30px;
  height: 30px;
  object-fit: cover;
  opacity: 0.4;
}

.github-icon:hover img {
  opacity: 0.4;
}
</style>    