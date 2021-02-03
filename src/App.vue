<template>
  <div id="app">
    <component :is="layout">
      <router-view/>
    </component>
  </div>
</template>

<script>
import './assets/scss/index.scss';
import Empty from '@/layouts/Empty'

export default {
  name: 'App',
  components:{
    Empty
  },
  computed:{
    layout(){
      return this.$route.meta.layout
    }
  }
}
</script>

<style lang="scss" scoped>

</style>
