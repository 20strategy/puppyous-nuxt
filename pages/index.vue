<template>
  <div class="container">
     
    <div class="container">
       <lazy-component>   
      <div class="columns is-vcentered is-multiline">
        <div class="column hero-body is-half-tablet is-one-half-widescreen">
          <div class="indexhero-text has-text-centered">
              <h1 class="title is-size-2">Walk and Play ♬</h1>
              <div class="content"> 
                <p>You love your puppy and you love your walk. Walking will bring your dog and you closer everyday. Let's make walking more pleasant.</p>
              </div>
              <nuxt-link to="/products" class="button is-twitter">
               Shop Now
              </nuxt-link>
          </div>
        </div>
       
        <div class="column is-half-tablet is-one-half-widescreen"> 
            <img
              src="https://images.ctfassets.net/80g45033v4tf/1f8itQq4arrIR2OCT6wJHO/8e0ff37a7e2149e84fd94bc1e2feb35e/homepage_1080_andblank_harness_and_model.jpg"
              alt="A model is holding a puppy" />
           
        </div> 
       
      </div>  
       </lazy-component>
    </div>
    
    <div class="full-width">
      <section class="section">
          <div class="container">
            <div class="columns">
              <div class="column">
                <h2 class="has-text-centered has-text-weight-light">Welcome to puppyous.com</h2>
              </div>
            </div>
        </div>
      </section>
    <lazy-component
      v-for="collection in collections"
      :key="collection.id"
      class="columns collection-banner">
      <div class="column">
        <nuxt-link :to="`/collection/${collection.handle}`">
          <section class="hero is-large is-info">
            <div
              class="hero-body"
              :style="{
                backgroundImage:`url(${collection.image.src})`,
                backgroundPosition: 'center',
                backgroundSize: 'cover'}">
                    <div class="container">
                        <h1 class="title is-shadow"> {{ collection.title }}</h1>
                    </div>
            </div>
          </section>
        </nuxt-link>
      </div>
    </lazy-component>
    </div>


    
  </div>
</template>

<script>


export default {
  
  asyncData ({ app }) {
    components: [

    ]
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
.indexhero-text{
  width: 90%;
  margin-left:auto;
  margin-right:auto;
}
</style>
