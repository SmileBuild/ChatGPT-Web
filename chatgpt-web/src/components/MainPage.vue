<template>
  <el-container>
    <el-main>
      <Hero />
      <Editor />
    </el-main>
  </el-container>
</template>

<script>
import Editor from './Editor.vue'
import Hero from './Hero.vue'
export default {
  name: 'MainPage',
  components: {
    Editor,
    Hero
  }
}
</script>

<style lang="scss" scoped>

</style>