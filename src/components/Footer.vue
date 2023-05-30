<template>
  <footer class="footer">
    <span>all rights reserved</span>
  </footer>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Footer'
})
</script>

<style lang="scss" scoped>
  .footer {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 76px;

    span {
      font-style: normal;
      font-weight: 400;
      font-size: 13px;
      line-height: 16px;
      color: #000000;
    }
  }
</style>