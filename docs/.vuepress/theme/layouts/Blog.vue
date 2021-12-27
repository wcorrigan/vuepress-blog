<template>
  <div>
    <div class="content">
      <h1>{{ $page.frontmatter.title }}</h1>
      <Content/>
    </div>
    <div v-if="posts.length">
      <div class="columns">
        <!-- Filter controls -->
        <div class="column is-3">
          <nav class="panel">
            <p class="panel-heading">Categories</p>
            <a
              class="panel-block is-capitalized"
              :class="{'is-active':i == currentCat}"
              v-for="cat, i in findTaxonomy('categories')"
              @click.prevent="filterTaxonomies('categories', cat);currentCat = i;currentTag = null"
            >
              <span class="panel-icon">
                <i class="fas fa-tasks"></i>
              </span>
              {{cat}}
            </a>
          </nav>
          <nav class="panel">
            <p class="panel-heading">Tags</p>
            <div class="tags panel-block is-capitalized">
              <a
                v-for="tag, i in findTaxonomy('tags')"
                @click.prevent="filterTaxonomies('tags', tag);currentTag = i;currentCat = null"
              >
                <span class="tag" :class="{'has-text-link':i == currentTag}">{{tag}}</span>
              </a>
            </div>
          </nav>
        </div>
        <!-- Blog posts -->
        <div class="column is-9">
          <transition-group tag="ul" name="posts">
            <li class="box" v-for="post in posts" :key="post.path">
              <router-link :to="post.path">
                <div class>
                  <img v-if="post.frontmatter.image" :src="$withBase(post.frontmatter.image)">
                </div>
                <p class="title is-3">{{post.frontmatter.title}}</p>
                <p class="subtitle is-6">
                  <span class="icon">
                    <i class="fas fa-calendar-alt"></i>
                  </span>
                  {{post.frontmatter.date | formatDate}}
                  <span class="icon">
                    <i class="fas fa-tags"></i>
                  </span>
                  <span v-for="tag in post.frontmatter.tags" class="tag">{{tag}}</span>
                </p>
              </router-link>
            </li>
          </transition-group>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      posts: [],
      currentCat: null,
      currentTag: null
    };
  },
  created() {
    this.posts = this.allPosts;
  },
  computed: {
    allPosts() {
      let currentCollection = this.$page.path;
      let posts = this.$site.pages
        .filter(page => {
          return page.path.match(
            new RegExp(`(${currentCollection})(?=.*html$)`)
          );
        })
        .sort((a, b) => {
          return new Date(b.frontmatter.date) - new Date(a.frontmatter.date);
        });
      return posts;
    }
  },
  filters: {
    formatDate(value) {
      let d = new Date(value);
      return d.toDateString();
    }
  },
  methods: {
    filterTaxonomies(type, tax) {
      let currentCollection = this.$page.path;
      let posts = this.$site.pages.filter(post => {
        if (post.frontmatter[type]) {
          return post.frontmatter[type].includes(tax);
        }
      });
      this.posts = posts;
    },
    findTaxonomy(type) {
      let tax = [];
      let posts = this.$site.pages.forEach(function(post) {
        if (post.frontmatter[type]) {
          tax = tax.concat(post.frontmatter[type]);
        }
      });
      let uniqTax = [...new Set(tax)];
      return uniqTax;
    }
  }
};
</script>

<style>
.tag {
  margin: 0 0.3em;
}

.posts-move {
  transition: transform 1s;
}

.posts-enter-active,
.posts-leave-active {
  transition: all 0.5s;
}

.posts-enter,
.posts-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style>