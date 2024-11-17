<template>
  <div class="hello">
    <button @click="debounceFn()">防抖</button>
    <button @click="throttleFn()">节流</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      debounceTimer: null,
      throttleTimer: null,
    };
  },
  methods: {
    // 防抖函数
    debounceFn() {
      clearTimeout(this.debounceTimer);  // 清除之前的定时器
      this.debounceTimer = setTimeout(() => {
        console.log("防抖触发：123");
      }, 2000);
    },

    // 节流函数
    throttleFn() {
      if (this.throttleTimer) return;  // 如果定时器存在，直接返回
      this.throttleTimer = setTimeout(() => {
        console.log('节流触发');
        this.throttleTimer = null;  // 释放定时器
      }, 200);
    },
  },
};
</script>

<style scoped>
/* 添加样式（如果需要） */
</style>
