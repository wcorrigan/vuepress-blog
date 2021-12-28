<template>
  <div class="container">
    <Navbar/>
    <Component :is="currentLayout"/>
    <Footer/>
  </div>
</template>

<script>
import Home from "./layouts/Home.vue";
import Blog from "./layouts/Blog.vue";
import Post from "./layouts/Post.vue";
export default {
  components: {
    Home,
    Blog,
    Post
  },
  computed: {
    currentLayout() {
      const { path } = this.$page;
      if (path === "/") {
        return "Home";
      } else if (path.endsWith("/")) {
        return "Blog";
      } else if (path.endsWith(".html")) {
        return "Post";
      }
    }
  }
};
</script>
<style src="prismjs/themes/prism-tomorrow.css"></style>
<style lang="stylus">
/* colors */
$accentColor = #3eaf7c;
$textColor = #2c3e50;
$borderColor = #eaecef;
$codeBgColor = #282c34;
$arrowBgColor = #ccc;

/* code */
$lineNumbersWrapperWidth = 3.5rem;
$codeLang = js ts html md vue css sass scss less stylus go java c sh yaml py;

@import './styles/custom-blocks.styl';
@import './styles/code.styl';
</style>