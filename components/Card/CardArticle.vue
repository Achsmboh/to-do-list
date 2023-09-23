<template>
  <div class="card" style="width: 18rem">
    <img :src="article.img" class="card-img-top" :alt="article.img" />
    <div class="card-body">
      <h5 class="card-title">
        {{ article.title }}
      </h5>
      <p class="card-text">
        {{ article.content }}
      </p>
      <nuxt-link class="btn btn-primary" :to="'detail/' + article.title"
        >Baca Selengkapnya</nuxt-link
      >
    </div>
  </div>
</template>
<script>
export default {
  props: {
    article: {
      type: Object,
      default: () => {},
    },
  },
}
</script>
