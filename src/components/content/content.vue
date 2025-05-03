<!-- 主页的主要内容 -->

<template>
  <div class="post-item">
    <div class="post-image">
      <img :src="postimage" alt="test" class="image" loading="lazy" @error="handleImageError">
    </div>
    <div class="post-content">
      <h2 class="post-title">{{ title }}</h2>
      <p class="content-text">{{ truncatedContent }}</p>
      <div class="post-meta">
        <p class="time-text">时间</p>
        <p class="comment-count">评论数</p>
        <p class="like-count">点赞数</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import postimage from '../../assets/ep.png'
const originalContent = '爱马仕表示， 计划从5月份开始，在常规涨价6 - 7%的基础上，进一步全面上调美国市场的定价，以抵消特朗普关税政策带来的成本。 4月17日，爱马仕发布2025年第一季度财报，报告期内该集团收入达到41.29亿欧元，按当前汇率同比增长9%，按恒定汇率则增长7%。 爱马仕的增长已出现大幅放缓。 2024年全年，爱马仕总营收达到152亿欧元，按固定汇率计算同比增长15%，2024年第一季度则上涨17%。'
const truncatedContent = originalContent.length > 70 ? originalContent.slice(0, 70) + '...' : originalContent;
const title = '爱马仕市场动态与财务状况'; 

const handleImageError = () => {
  // 可以在这里添加图片加载失败后的处理逻辑，如显示默认图片
  console.log('图片加载失败');
};
</script>

<style scoped>
.image {
  width: 100%;
  height: auto;
  box-shadow: 5px 5px 10px rgba(0, 0, 0, 0.3);
}

.post-item {
  display: flex;
  justify-content: space-between;
  box-shadow: 5px 5px 10px rgba(0, 0, 0, 0.1);
  border: 1px solid rgba(253,239,242,0.5);
  min-height: 200px; /* 使用更灵活的高度设置 */
  max-height: 300px;
  overflow: hidden;
}

.post-image {
  width: 40%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.post-title {
  color: #333;
  font-size: 1.2em;
  margin-bottom: 5px;
}

.post-content {
  width: 50%;
  display: flex;
  flex-direction: column;
  min-height: 0;
}

.content-text {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  overflow: hidden;
  text-overflow: ellipsis;
  margin-bottom: 10px;
}

.post-meta {
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin-top: auto;
}
</style>    