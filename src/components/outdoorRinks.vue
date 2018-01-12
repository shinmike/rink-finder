<template>
  <div class="sidebar limit fl w-40 pa2">
    <h2>Outdoor Ice Rinks</h2>
    <div v-for="info in outdoorRinkData">
        <article class="grow shadow-2 mw5 center bg-white br3 pa3 pa4-ns mv3 ba b--black-10">
          <div class="tc">
            <h1 class="f4">{{info.name}}</h1>
            <hr class="mw3 bb bw1 b--black-10">
          </div>
          <p class="tc f6 black-70">
            {{info.friendly_location}}
          </p>
        </article>
      </div>
  </div>
</template>

<script>
export default {
  props: ["outdoorRinkData"],
  data() {
    return {};
  },
  methods: {}
};
</script>

<style>
.sidebar {
    box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.3);
    background: #f7f8f9;
    overflow-y: scroll;
}

</style>