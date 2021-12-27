<template>
  <nav class="navbar is-dark" role="navigation" aria-label="main navigation">
    <div class="navbar-brand is-marginless">
      <a class="navbar-item" :href="$withBase('/')">
        <span class="icon is-large has-text-warning">
          <i class="fas fa-2x fa-laptop-code"></i>
        </span>
      </a>
      <a
        role="button"
        class="navbar-burger"
        :class="{'is-active': isActive}"
        @click="isActive = !isActive"
        aria-label="menu"
        aria-expanded="false"
        data-target="navbarMenu"
      >
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
      </a>
    </div>

    <div id="navbarMenu" class="navbar-menu" :class="{'is-active': isActive}">
      <div class="navbar-end">
        <a class="navbar-item" :href="$withBase('/')">HOME</a>
        <a class="navbar-item" :href="$withBase('/blog/')">BLOG</a>
        <a
          class="navbar-item"
          :href="$withBase(collection.path)"
          v-for="collection in collections"
        >{{collection.path | formatNavItems}}</a>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  data: function() {
    return {
      isActive: false
    };
  },
  computed: {
    collections() {
      let pages = this.$site.pages.filter(page => {
        return page.path.match(/(blog\/).+(\/$)/);
      });
      return pages;
    }
  },
  filters: {
    formatNavItems(value) {
      if (!value) return "";
      let pos = value.search(/\w+\/$/);
      let res = value.slice(pos, value.length - 1);
      return res.toUpperCase();
    }
  }
};
</script>

<style>
.navbar {
  padding-right: 1em;
}
</style>