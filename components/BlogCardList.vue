<template>
  <div class="columns is-multiline">
    <div
      v-for="article in blog.articles.edges"
      :key="article.node.id"
      class="column is-full-mobile is-half-tablet is-one-third-desktop"
    >
      <nuxt-link :to="`/blog/${blog.handle}/article/${article.node.handle}`">
        <div class="card">
          <div class="card-image">
            <figure >
              <img
                :src="article.node.image.originalSrc"
                :alt="article.node.image.altText"
              >
            </figure>
          
          </div>
          <div class="card-content">
            <p class="title is-4">
              {{ article.node.title }}
            </p>
            <p class="subtitle is-6">{{ article.node.excerpt }}</p>
            <div class="content">
              <b-taglist class="is-marginless">
                <b-tag
                  v-for="tag in article.node.tags"
                  :key="tag"
                  href="#"
                  type="is-primary"
                >
                  #{{ tag }}
                </b-tag>
              </b-taglist>
              
              <p class="tag is-rounded is-info has-text-left">{{ $dayjs(article.node.publishedAt, $dayjs.ISO_8601).format('MMMM D, YYYY') }}</p>
            
            
            </div>
          </div>
        </div>
      </nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BlogCardList',
  props: {
    blog: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
  .subtitle {
    height: 20px;
    overflow: hidden;
    text-overflow: ellipsis;
    margin-bottom: 1rem;
  }
  figure.is-flex {
    justify-content: center;
  }
</style>
