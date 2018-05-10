<template>
  <div class="search-result">
      <a :href="url">
          <img :src="imgurl" :alt="name" />
          <br />
          {{name}}
          <br />
        </a>
        <p class="summary">{{summary}}</p>
  </div>
</template>

<script lang="ts">
    import { Component, Prop, Vue } from 'vue-property-decorator';

    @Component({
        name: 'SearchResult'
    })
    
    export default class SearchResult extends Vue {
        @Prop() name: string
        @Prop() url: string
        @Prop() imgurl: string
        @Prop() summary: string
        
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.summary {
    display: none;
}
</style>