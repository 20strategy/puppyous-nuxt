<template>
<div class="container">
  <b-navbar>
    <template slot="brand">
      <b-navbar-item tag="nuxt-link" to="/">
       <p class="logo-font">Puppy<span class="ous">Ous</span></p>
      </b-navbar-item>
    </template>
    <template slot="start">
      <b-navbar-item
        tag="nuxt-link"
        to="/about"
      >
       About
      </b-navbar-item>
      <b-navbar-item
        tag="nuxt-link"
        to="/products"
      >
       Shop
      </b-navbar-item>
       <b-navbar-item
        tag="nuxt-link"
        to="/collection"
      >
        Collections
      </b-navbar-item>
      <b-navbar-dropdown label="Blog">
        <b-navbar-item
          v-for="blog in blogs"
          :key="blog.id"
          tag="nuxt-link"
          :to="`/blog/${blog.node.handle}`"
        >
          {{ blog.node.title }}
        </b-navbar-item>
      </b-navbar-dropdown>       
    </template>

    <template slot="end">
      <b-navbar-item tag="div">
        <div class="buttons">
          <nuxt-link to="/cart" class="button is-primary">
            <b-icon
              icon="cart"
            />
            <span>{{ cardCount }}</span>
          </nuxt-link>
        </div>
      </b-navbar-item>
    </template>
  </b-navbar>
  </div>
</template>

<script>
export default {
  name: 'TheNavbar',
  computed: {
    cardCount () {
      return this.$store.state.checkout.lineItems.length
    },
    blogs () {
      return this.$store.state.blogs
    }
  },
  methods: {
  }
}
</script>

<style scoped>
.logo-font {
  margin-bottom:3px;
  font-size:2rem;
  font-weight:bold;
  color:rgb(82, 82, 245);
  letter-spacing:2px;
  margin-right:10px;
}
.ous{
  color:tomato;
  
}
.container {
  padding-top: 20px;
  padding-right: 10px;
  padding-left: 10px;
  height: 100px;}

a.navbar-link.is-active {
  background-color: red;
}

</style>
