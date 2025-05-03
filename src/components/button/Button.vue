<template>
    <button
      class="button"
      :class="{ 'active': isActive }"
      @click="toggleState"
    >
      {{ isActive ? text1 : text0 }}
    </button>
  </template>
  
  <script setup>
  import { ref, defineProps } from 'vue';
  import './button.css'
  
  const props = defineProps({
    text1: {
      type: String,
      default: '已激活状态'
    },
    text0: {
      type: String,
      default: '未激活状态'
    }
  });
  
  const isActive = ref(false);
  
  const toggleState = () => {
    isActive.value =!isActive.value;
  };
  </script>
  
<style scoped>

</style>    