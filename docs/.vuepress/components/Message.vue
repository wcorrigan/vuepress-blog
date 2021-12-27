<template>
  <article class="message is-info" :class="{hidden: dismiss}">
    <div class="message-header">
      <p>Welcome to Our Blog</p>
      <button @click="hide" class="delete"></button>
    </div>
    <div class="message-body">Lorem ipsum dolor sit amet...</div>
  </article>
</template>

<script>
export default {
  data: function() {
    return {
      dismiss: false
    };
  },
  methods: {
    hide() {
      this.dismiss = true;
    }
  }
};
</script>

<style>
.hidden {
  display: none;
}
</style>