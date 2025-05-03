<template>
    <div ref="blogHomeContainer" class="blog-home-container">
        <Nav2 />
        <div class="main-section">
            <h1 class="blog-title">霜印繁星 の BLOG</h1>
            <div class="scroll-down-icon">
                <img :src="scrollIconSrc" alt="Scroll Down Icon">
            </div>
        </div>
        <div >
            <TechHome/>
        </div>
        <div>
            
        </div>
    </div>

</template>

<script setup>
import { ref, onMounted } from 'vue';

import Nav2 from './nav2.vue';
import TechHome from './tech-home.vue';
import TechSelect from './button/tech-select.vue';
import Search from './search.vue';

// 背景图片路径
const bgImageSrc = ref(new URL('../assets/bg.png', import.meta.url).href);
// 下滑提示图标路径
const scrollIconSrc = ref(new URL('../assets/down-end.png', import.meta.url).href);
const blogHomeContainer = ref(null);
const isTop = ref(true);
const handleScroll = () => {
  if (window.scrollY === 0) {
    isTop.value = true;
  } else {
    isTop.value = false;
  }
};

onMounted(() => {
    if (blogHomeContainer.value) {
        blogHomeContainer.value.style.backgroundImage = `url(${bgImageSrc.value})`;
        // blogHomeContainer.value.style.backgroundPosition = 'center';
        blogHomeContainer.value.style.backgroundAttachment = 'fixed';
    }
    window.addEventListener('scroll', handleScroll);
});
</script>

<style scoped>
.blog-home-container {
    min-height: 100vh;
    height: 100%;
    width: 100%;
    color: white;
    background-size: cover;
    z-index:1; 
    position: relative;
}   

.nav-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1% 2%;
    width: 100%;
}

.nav-links {
    list-style-type: none;
    padding: 0;
    margin: 0;
    display: flex;
}

.nav-links li {
    margin-left: 15px;
    cursor: pointer;
}

.main-section {
    text-align: center;
    padding: 10% 0 0; 
    width: 100%;
    height: 90vh;
    position: relative;
}

.blog-title {
    font-size: 40px;
}
.scroll-down-icon {
    position: absolute;
    top: 80%;
    left: 50%;
    transform: translateX(-50%);
    width: 20px; 
    height: 20px; 
    animation: bounce 2s infinite;
    display: flex;
    justify-content: center;
    align-items: center;
}

.scroll-down-icon img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0.4;
}



@keyframes bounce {
    0%, 20%, 50%, 80%, 100% {
        transform: translateX(-50%) translateY(0);
    }
    40% {
        transform: translateX(-50%) translateY(-20px);
    }
    60% {
        transform: translateX(-50%) translateY(-10px);
    }
}

.content{
    width: 95%;
}

</style>    