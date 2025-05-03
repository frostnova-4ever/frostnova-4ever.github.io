<template>
    <div ref="blogHomeContainer" class="blog-home-container">
        <div class="layout-container tech-select-bgc">
            <div>
                <TechSelect :text="techOptions"/>
            </div>
            <div class="search">
                <List/>
                <Search/>
            </div>
        </div>
        <div class="layout-container" >
            <div class="left-panel">
                <Self/>
                <newContent/>
                <!-- 左侧屏内容 -->
                <p>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                    <P>ASFBSAKJLDF</P>
                </p>
            </div>
            <div class="middle-panel">
                <Content/>
            </div>
            <div class="right-panel">
                <datetime/>
                <views/>
                <!-- 右侧屏内容 -->
                <p>右侧屏内容</p>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import TechSelect from './button/select.vue';
import Search from './search.vue';
import List from './list.vue';
import Content from './content/content.vue';
import Self from './content/self.vue';
import newContent from './content/newContent.vue';
import datetime from './content/datetime.vue';
import views from './content/views.vue';

// 定义存储后端数据的响应式变量
const techOptions = ref([]);

// 从本地 JSON 文件加载数据
onMounted(async () => {
    try {
        const response = await axios.get('src\\json\\components.json'); // 访问 public 文件夹中的 JSON 文件
        techOptions.value = response.data.options;
    } catch (error) {
        console.error('获取数据失败:', error);
    }
});
</script>

<style scoped>
.blog-home-container{
    margin-top:10px;
}

.layout-container {
    padding:0.5rem;
    display: flex;
    width: 97%;
    margin:0 auto;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    margin-top:10px;
    border-radius: 0.25rem;
    justify-content: space-between;
}
.tech-select-bgc{
    background-color: rgb(255, 255, 255,0);
}
.search{
    display: flex;
    gap: 10px;
}


.left-panel {
    flex: 1;
    background-color:rgba(0, 0, 0, 0);
    padding: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.middle-panel {
    flex: 3; 
    background-color: white;
    color: black;
    padding: 10px;
}

.right-panel {
    flex: 1; 
    background-color:rgba(0, 0, 0, 0);
    padding: 10px;
}
</style>