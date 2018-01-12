<template>
  <div class="avenir black-80 blah">
    <search></search>
  </div>
</template>

<script>
import search from "./components/search.vue";

export default {
  components: {
    search: search
  },
  data() {
    return {};
  }
};
</script>

<style>
html, body, .google-map, .sidebar, .search, .blah{
    height: 100%;
    margin: 0;
    padding: 0;
}

.google-map {
    position: relative;
}

</style>
