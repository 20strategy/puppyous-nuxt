<template>
  <div>
    <section class="hero is-info is-medium is-bold">
      <div
        class="hero-body"
        :style="{
          backgroundImage: (article.image === null) ? '' : `url(${article.image.originalSrc})`,
          backgroundPosition: 'center',
          backgroundSize: 'cover'
        }"
      />
    </section>
    <section class="articles section">
      <div class="container">
        <div class="card article">
          <div class="card-content">
            <div class="media">
              <div class="media-content has-text-centered">
                <h1 class="title article-title">{{ article.title }}</h1>
                
                  <span class="tag is-rounded is-info">{{ $dayjs(article.publishedAt, $dayjs.ISO_8601).format('MMMM D, YYYY') }}</span>
               
              </div>
            </div>
            <div class="content article-body">
              <p v-html="article.contentHtml" />
              <p class="tag is-rounded is-info">@{{ article.authorV2.name }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'Index',
  asyncData ({ app, params }) {
    return app.$axios({
      method: 'POST',
      data: {
        query: `query {
                  blogByHandle (handle:"${params.handle}") {
                    articleByHandle (handle: "${params.articleHandle}") {
                      title,
                      publishedAt,
                      contentHtml
                      authorV2 {
                        name
                      },
                      seo {
                        title,
                        description
                      }
                      image (maxWidth: 1200) {
                        altText,
                        originalSrc
                      }
                    }
                  }
                }`
      }
    }).then((response) => {
      return {
        article: response.data.data.blogByHandle.articleByHandle
      }
    })
  },
  head () {
    return {
      title: this.article.seo.title,
      meta: [
        { hid: 'description', name: 'description', content: this.article.seo.description }
      ]
    }
  }
}
</script>

<style scoped>
  .articles {
    margin: -200px 0 5rem;
  }
  .articles .content p {
    line-height: 1.9;
    margin: 15px 0;
  }
  .media-content {
    margin-top: 3rem;
  }
  .article {
    margin-top: 6rem;
  }
  @media screen and (min-width: 768px) {
    .article-body {
      margin: 0 6rem;
    }
  }
  .article-body {
    line-height: 1.4;
  }
  .hero-body {
    height: 500px;
  }
</style>
