<template>
  <div>
    <nav-menu></nav-menu>
    <h2 style="text-align: center;"> 新闻 </h2>
    <div class="news-div">
      <span v-for="article in articles" :key="article.title">
        <news-card :article="article"></news-card>
      </span>
      <news-card :article="article" style="visibility: hidden;"></news-card>
      <news-card :article="article" style="visibility: hidden;"></news-card>
      <news-card :article="article" style="visibility: hidden;"></news-card>
      <news-card :article="article" style="visibility: hidden;"></news-card>
      <news-card :article="article" style="visibility: hidden;"></news-card>
    </div>
  </div>
</template>

<script>
import navMenu from "@/components/navmenu.vue";
import newsCard from "@/components/newsCard.vue";
export default {
  name: "newsPage",
  components: {
    'nav-menu': navMenu,
    'news-card': newsCard, 
  },
  data() {
    return {
      news: [
        '11111', 
        '22222' 
      ], 
      articles: [{
        title: '456', 
        description: '123456789', 
        publishedAt: '1234-56-78',
        detail:'xijie'
      }, {
        title: '789', 
        description: '123456789', 
        publishedAt: '1234-56-78',
        detail:'xijie'
      }]
    }
  }
}
</script>


<style scoped>
.news-div {
  display: flex; 
  flex-wrap: wrap;
  justify-content: center;
}
</style>