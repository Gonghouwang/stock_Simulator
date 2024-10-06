<template>
  <div>
    <nav-menu></nav-menu>
    <p> 这里是首页 </p>
  </div>
</template>

<script>
import navMenu from "@/components/navmenu.vue";
export default {
  name: "homePage",
  components: {
    'nav-menu': navMenu
  },
}
</script>


<style scoped>

</style>