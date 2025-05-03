<template>
    <div>
      <!-- 分类索引 -->
      <div class="index-item">
        <span class="index-label">分类:</span>
        <span
          v-for="(category, index) in categories"
          :key="index"
          :class="{ 'active': selectedCategory === category }"
          @click="selectCategory(category)"
        >
          {{ category }}
        </span>
        <span v-if="categories.length > 10" class="more-btn" @click="showMoreCategories">更多</span>
      </div>
      <!-- 格式索引 -->
      <div class="index-item">
        <span class="index-label">格式:</span>
        <span
          v-for="(format, index) in formats"
          :key="index"
          :class="{ 'active': selectedFormat === format }"
          @click="selectFormat(format)"
        >
          {{ format }}
        </span>
      </div>
      <!-- 排序索引 -->
      <div class="index-item">
        <span class="index-label">排序:</span>
        <span
          v-for="(sort, index) in sorts"
          :key="index"
          :class="{ 'active': selectedSort === sort }"
          @click="selectSort(sort)"
        >
          {{ sort }}
        </span>
      </div>
      <!-- 数据展示区域 -->
      <div class="data-display">
        <div
          v-for="(item, key) in filteredData"
          :key="key"
          class="data-item"
        >
          {{ item.title }}
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  
  // 分类索引数据
  const categories = ['全部', '漂浮素材', '效果元素', '卡通手绘', '装饰图案', '图标元素', '促销标签', '边框纹理', '不规则图形', '表情包213123', '表情包2323', '表情包1111', '表情包3333', '表情包444'];
  // 格式索引数据
  const formats = ['全部', 'PSD', 'AI', 'EPS'];
  // 排序索引数据
  const sorts = ['推荐', '昨日热门', '最新上传', '热门下载', '热门收藏'];
  
  // 当前选中的分类
  const selectedCategory = ref('全部');
  // 当前选中的格式
  const selectedFormat = ref('全部');
  // 当前选中的排序
  const selectedSort = ref('推荐');
  
  // 模拟数据列表
  const dataList = [
    { title: '11', category: '全部', format: '全部', sort: '推荐' },
    { title: '22', category: '漂浮素材', format: 'PSD', sort: '昨日热门' },
    { title: '33', category: '效果元素', format: 'AI', sort: '最新上传' },
    { title: '44', category: '卡通手绘', format: 'EPS', sort: '热门下载' },
    { title: '55', category: '装饰图案', format: '全部', sort: '热门收藏' },
    { title: '66', category: '图标元素', format: 'PSD', sort: '推荐' }
  ];
  
  // 计算筛选后的数据
  const filteredData = computed(() => {
    return dataList.filter(item => {
      return (
        (selectedCategory.value === '全部' || item.category === selectedCategory.value) &&
        (selectedFormat.value === '全部' || item.format === selectedFormat.value) &&
        (selectedSort.value === '推荐' || item.sort === selectedSort.value)
      );
    });
  });
  
  // 选择分类的方法
  const selectCategory = (category) => {
    selectedCategory.value = category;
  };
  
  // 选择格式的方法
  const selectFormat = (format) => {
    selectedFormat.value = format;
  };
  
  // 选择排序的方法
  const selectSort = (sort) => {
    selectedSort.value = sort;
  };
  
  // 控制分类索引更多选项显示隐藏
  const showMoreCategories = () => {
    // 这里可添加逻辑控制更多分类的显示隐藏，比如切换一个控制变量
    console.log('点击了更多分类');
  };
  </script>
  
  <style scoped>
  .index-item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }
  
  .index-label {
    margin-right: 10px;
  }
  
  .index-item span {
    cursor: pointer;
    padding: 5px 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    background-color: white;
    margin-right: 5px;
    white-space: nowrap;
  }
  
  .index-item span.active {
    background-color: #007BFF;
    color: white;
    border-color: #007BFF;
  }
  
  .more-btn {
    cursor: pointer;
    color: #666;
  }
  
  .data-display {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }
  
  .data-item {
    width: 200px;
    height: 150px;
    border: 1px solid #f44336;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #333;
  }
  </style>