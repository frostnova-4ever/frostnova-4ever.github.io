<template>
  <div class="radio-container">
    <div
      v-if="radioOptions.length > 0"
      v-for="option in radioOptions"
      :key="option"
      class="button"
      :class="{ 'active': selected === option }"
      @click="selectOption(option)"
    >
      <span>{{ option }}</span>
    </div>
    <!-- 空数组提示 -->
    <div v-else class="no-options">暂无选项</div>
  </div>
</template>

<script setup>
import { ref, defineProps, computed } from 'vue';
import './Button.css'

const props = defineProps({
  text: {
    type: Array,
    default: () => []
  }
});

const radioOptions = computed(() => props.text);
const selected = ref(radioOptions.value.length > 0? radioOptions.value[0] : null);

const selectOption = (option) => {
  selected.value = option;
};
</script>

<style scoped>
.radio-container {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
  padding: 0rem;
  gap: 0.5rem; /* 添加间距 */
}

.no-options {
  color: #999;
  font-size: 1.25rem;
}
</style>    