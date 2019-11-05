<template></template>


<style lang="stylus" scoped></style>


<script>
export default {
  name: "keep-alive",
  abstract: true, // 判断当前组件虚拟dom是否渲染成真实dom的关键
  props: {
    include: patternTypes, // 缓存白名单
    exclude: patternTypes, // 缓存黑名单
    max: [String, Number] // 缓存的组件
  },
  created() {
    this.cache = Object.create(null); // 缓存虚拟dom
    this.keys = []; // 缓存的虚拟dom的键集合
  },
  destroyed() {
    for (const key in this.cache) {
      // 删除所有的缓存
      pruneCacheEntry(this.cache, key, this.keys);
    }
  },
  mounted() {
    // 实时监听黑白名单的变动
    this.$watch("include", val => {
      pruneCache(this, name => matched(val, name));
    });
    this.$watch("exclude", val => {
      pruneCache(this, name => !matches(val, name));
    });
  },

  render() {
    // 先省略...
  }
};
</script>