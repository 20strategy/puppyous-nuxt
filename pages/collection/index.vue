<template class="container">
   <div class="full-width">
    <section class="section">
      <div class="container">
      
           <h2 class="has-text-centered">puppyous collections</h2>
       
      </div>
    </section>
    <lazy-component>
    <div
      v-for="collection in collections"
      :key="collection.id"
      class="columns collection-banner"
    >
      <div class="column">
        <nuxt-link :to="`/collection/${collection.handle}`">
          <section class="hero is-large is-info">
            <div
              class="hero-body"
              :style="{
                backgroundImage: (collection.image === null) ? '' : `url(${collection.image.src})`,
                backgroundPosition: 'center',
                backgroundSize: 'cover'
              }"
            >
              <div class="container">
                <h1 class="title is-shadow">
                  {{ collection.title }}
                </h1>
              </div>
            </div>
          </section>
        </nuxt-link>
      </div>
    </div></lazy-component>
  </div>
</template>

<script>
export default {
  
asyncData ({ app }) {
    return app.$axios({
      method: 'POST',
      data: {
        query: `query {
              shop {
                name,
                description
              },
              blogs(first: 10) {
                edges {
                    node {
                      handle,
                      title
                    }
                  }
              }
            }`
      }
    }).then((response) => {
      return {
        description: response.data.data.shop.description,
        shop: response.data.data.shop,
        blogs: response.data.data.blogs.edges
      }
    })
  },
  data () {
    return {
      collections: []
    }
  },
  created () {
    this.$shopify.collection.fetchAllWithProducts().then((collections) => {
      this.collections = collections
    })
    this.$store.commit('setBlogs', this.blogs)
    this.$store.commit('setShop', this.shop)
  },
  beforeUpdate () {
    if (typeof this.$store.state.checkout.id === 'undefined') {
      this.$shopify.checkout.create().then((checkout) => {
        this.$store.commit('setCheckout', checkout)
      })
    }
  },
  head () {
    return {
      title: this.shop.name,
      meta: [
        { hid: 'description', name: 'description', content: this.description }
      ]
    }
  }
}
</script>

<style>
.collection-banner {
  margin-bottom: 2rem;
  margin-top: 2rem !important;
}
</style>