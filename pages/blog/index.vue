<template>
  <div class="container">
    <h1>{{ title }}</h1>
    <h2>{{ description }}</h2>
    <!-- <p>❗ Blog page is under construction ❗</p> -->

    <!-- search bar -->
    <section class="field columns is-centered">
      <div class="control has-icons-left column is-3-widescreen is-4-tablet">
        <input
          @keyup="searchPosts(keyword)"
          v-model="keyword"
          type="search"
          class="input is-rounded"
          placeholder="Press ↵ to search"
        />
        <i class="icon is-small is-left icon-search"></i>
      </div>
    </section>

    <!-- blog posts -->
    <section class="posts columns is-multiline is-centered">
      <article
        v-for="post in allBlog_postss.edges"
        :key="post.node._meta.id"
        class="box column is-3-widescreen is-4-tablet"
      >
        <nuxt-link :to="`/blog/${post.node._meta.uid}/`">
          <img class="post-image" :src="post.node.image.url" :alt="post.node.image.alt" />
          <p class="title is-6">{{post.node.title[0].text}}</p>
        </nuxt-link>
      </article>
      <p></p>
    </section>

    <!-- I want to PAGINATE my posts here -->
    <section class="columns is-multiline is-centered is-mobile">
      <button
        v-if="allBlog_postss.pageInfo.hasNextPage"
        class="button is-black"
        @click="loadMorePosts(keyword, allBlog_postss.pageInfo.endCursor)"
      >Show More</button>
    </section>
  </div>
</template>

<script>
import gql from 'graphql-tag'

const queryPost = gql`
  query($fulltext: String, $cursor: String) {
    allBlog_postss(
      lang: "en-us"
      fulltext: $fulltext
      sortBy: date_DESC
      first: 9
      after: $cursor
    ) {
      pageInfo {
        hasNextPage
        hasPreviousPage
        startCursor
        endCursor
      }
      edges {
        node {
          title
          image
          _meta {
            uid
            id
          }
        }
      }
    }
  }
`

export default {
  data() {
    return {
      title: 'Blog',
      description:
        'Welcome to my blog. Browse through a streamline of tech tutorials that suits your needs.',

      keyword: '' // for searchPosts()
    }
  },

  head() {
    return {
      title: this.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.description
        },
        {
          hid: 'og:type', // open graph type
          name: 'og:type',
          content: 'blog'
        },
        {
          hid: 'apple-mobile-web-app-title', // open graph mobile web app title
          name: 'apple-mobile-web-app-title',
          content: this.title
        },
        {
          hid: 'og:title', // open graph title
          name: 'og:title',
          content: this.title
        },
        {
          hid: 'og:description', // open graph description
          name: 'og:description',
          content: this.description
        },
        {
          hid: 'og:url', // open graph url
          name: 'og:url',
          content: 'https://markperez.dev' + this.$route.fullPath
        },
        {
          hid: 'og:image', // open graph image
          name: 'og:image',
          content: 'https://markperez.dev/_nuxt/img/ec3ab3b.png'
        }
      ]
    }
  },

  apollo: {
    allBlog_postss: {
      query: queryPost,

      variables() {
        return {
          fulltext: '',
          cursor: ''
        }
      },

      fetchPolicy: 'cache-and-network'
    }
  },

  methods: {
    loadMorePosts(searchTerm, currentCursor) {
      this.$apollo.queries.allBlog_postss.fetchMore({
        variables: {
          fulltext: searchTerm,
          cursor: currentCursor
        },

        updateQuery: (previousResult, { fetchMoreResult }) => {
          if (!fetchMoreResult) return previousResult

          return {
            allBlog_postss: Object.assign({}, fetchMoreResult.allBlog_postss, {
              __typename: fetchMoreResult.allBlog_postss.__typename,
              edges: previousResult.allBlog_postss.edges.concat(
                fetchMoreResult.allBlog_postss.edges
              ),
              pageInfo: fetchMoreResult.allBlog_postss.pageInfo
            })
          }
        }
      })
    },

    searchPosts(searchTerm) {
      this.$apollo.queries.allBlog_postss.fetchMore({
        variables: {
          fulltext: searchTerm,
          cursor: ''
        },

        updateQuery: (previousResult, { fetchMoreResult }) => {
          if (!fetchMoreResult) return previousResult

          return {
            allBlog_postss: Object.assign({}, fetchMoreResult.allBlog_postss, {
              __typename: fetchMoreResult.allBlog_postss.__typename,
              edges: [...fetchMoreResult.allBlog_postss.edges],
              pageInfo: fetchMoreResult.allBlog_postss.pageInfo
            })
          }
        }
      })
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 30px;
  padding-top: 0.5rem;
}

h2 {
  margin: 2rem 0;
}

button {
  margin: 1rem;
}

/* add transitions to input boxes */
input {
  transition: all 0.2s;
  background-color: #edf2f7;
}

/* adopt color scheme to input boxes */
input:focus {
  transition: all 0.2s;
  border-color: #00c58e;
}

.column {
  padding: unset;
}

.posts {
  margin-top: 1rem;
  margin-bottom: 2rem;
}

.post-image {
  border-radius: 6px;
}

.container {
  padding: 0 2rem;
}

.box {
  transition: all 0.2s;
  margin: 1rem;
}

.box:hover {
  transition: all 0.2s;
  transform: scale(1.03);
  box-shadow: 0px 2px 18px 0px rgba(0, 0, 0, 0.3);
}

.title {
  text-align: center;
  padding: 1rem 5px;
}

.pagination {
  margin: 1rem;
}

.button {
  font-weight: bold;
  margin-bottom: 2rem;
}
</style>
 