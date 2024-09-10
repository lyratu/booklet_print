<script setup lang="ts">
import Content from './components/Content.vue'
import Footer from './components/Footer.vue'
</script>

<template>
  <div class="main">
    <Content />
    <Footer />
  </div>
</template>

<style lang="scss" scoped>
.main {
  display: flex;
  flex-direction: column;
}
</style>
