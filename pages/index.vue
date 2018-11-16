<template>
  <section class="wrap">
    <!-- <no-ssr>
      <Slideout 
        :toggle-selectors="['.toggle-button']" 
        menu="#menu" 
        panel="#panel" 
        @on-open="open">
        <nav id="menu">
          <div>Menu</div>
        </nav>
        <main id="panel">
          <header>
            <div>
              <button class="toggle-button">☰</button>
              Panel
            </div>
          </header>
        </main>
      </Slideout>
    </no-ssr> -->
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
  },
  methods: {
    open: function() {
      console.log('slideoutOpen')
    }
  }
}
</script>

<style lang="scss" scoped>
html {
  h1 {
    font-size: 18px;
    margin-bottom: 5px;
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
  .wrap {
    width: 95%;
    margin: 0 auto;
    .sr-container {
      margin-bottom: 20px;
    }
    .sr-url {
      color: #006621;
    }
  }
}

.slideout-menu {
  position: absolute;
  top: 25%;
  bottom: 0;
  width: 90vw;
  height: 193px;
  overflow-y: scroll;
  -webkit-overflow-scrolling: touch;
  z-index: 0;
  display: none;
  background-color: black;
  color: white;
}

.slideout-menu-left {
  left: 0;
}

.slideout-menu-right {
  right: 0;
}

.slideout-panel {
  background-color: transparent;
  color: black;
  position: relative;
  z-index: 1;
  will-change: transform;
}

.slideout-open,
.slideout-open body,
.slideout-open .slideout-panel {
  overflow: hidden;
}

.slideout-open .slideout-menu {
  display: block;
}
</style>
