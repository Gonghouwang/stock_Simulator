<template>
  <el-row>
    <el-menu router
             :default-active="this.$route.path"
             mode="horizontal"
             style="font-weight: bold; font-size: 20px"
             >
      <el-menu-item index="/">home</el-menu-item>
      <el-menu-item index="/login">Login</el-menu-item>
    </el-menu>
  </el-row>
</template>

<script>
export default {
  name: "navMenu"
}
</script>


<style scoped>

</style>