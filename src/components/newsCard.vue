<template>
    <div class="news-card">
      <h3>{{ article.title }}</h3>
      <p>{{ article.description }}</p>
      <small>{{ article.publishedAt }}</small> <br/>
      <button @click="viewDetails">查看详情</button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      article: Object,
    },
    methods: {
      viewDetails() {
        this.$emit('view-details', this.article);
      }
    }
  }
  </script>
  
  <style scoped>
  .news-card {
    border: 1px solid #ddd;
    padding: 16px;
    margin: 10px;
    border-radius: 8px;
    min-width: 400px;
    max-width: 400px;
  }
  </style>
  