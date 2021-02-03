<template>
  <div class="layout">
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'Empty'
}
</script>

<style lang="scss" scoped>
.layout{
  background: -webkit-linear-gradient(45deg, rgb(45, 32, 36), rgb(184, 57, 78));
  background: -moz-linear-gradient(45deg, rgb(45, 32, 36), rgb(184, 57, 78));
  background: linear-gradient(45deg, rgb(45, 32, 36), rgb(184, 57, 78));
  min-height: 100vh;
}
</style>