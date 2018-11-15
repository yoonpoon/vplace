<template>
  <section class="container">
    <masonry
      :cols="{default: 3, 1000: 2, 800: 1}"
      :gutter="{default: '30px', 700: '15px'}"
    >
      <div 
        v-for="post in posts" 
        :key="post.id"
        class="col s12 col m6 col l4 sr-container">
        <a 
          :href="post.fields.link"
          target="_blank"
          class="sr-link"> 
          <h1 class="sr-link-title"> {{ post.fields.title }} </h1> 
        </a>
        <h2 class="sr-url">{{ post.fields.link }}</h2>
        <p> {{ post.fields.intro }} </p>
      </div>
    </masonry>
  </section>
</template>

<script>
import client from '~/plugins/contentful'
import VueMasonry from '~/plugins/VueMasonry'

export default {
  data() {
    return {
      posts: []
    }
  },
  asyncData() {
    return client
      .getEntries({
        content_type: 'index',
        order: '-sys.createdAt',
        include: 4
      })
      .then(({ items: [{ fields }] }) => fields)
      .catch(console.error)
  }
}
</script>

<style lang="scss" scoped>
html {
  h1 {
    font-size: 18px;
    margin-bottom: 2.5px;
    margin-top: 20px;
    font-weight: lighter;
  }
  h2 {
    font-size: 14px;
    margin-bottom: 5px;
    font-weight: lighter;
  }
  a {
    color: #1a0dab;
  }
}
.container {
  .row {
    .sr-container {
      margin-bottom: 20px;
      p {
        line-height: 1.4;
      }
    }
    .sr-url {
      color: #006621;
    }
  }
}
</style>
