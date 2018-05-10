<template>
  <div class="search-panel">
      <input type="text" v-model="query"  v-on:keyup.enter="search" placeholder="Enter a Movie Title..."><br />
      <button class="btn btn-primary" @click="search">Search</button>
  </div>
</template>

<script lang="ts">
var urlRoot: string = 'http://api.tvmaze.com/search/shows?q=';
var defaultQuery: string = 'http://api.tvmaze.com/shows?page=1';

import { Component, Prop, Vue } from 'vue-property-decorator';

@Component({
    name: 'SearchPanel'
})

export default class SearchPanel extends Vue {
    query: string = '';

    search() {
        var url: string = '';
        var queryWasNull: boolean = true;
        if (this.query !== '') {
            var url: string = urlRoot + encodeURIComponent(this.query);
            queryWasNull = false;
        } else {
            var url: string = defaultQuery;
        }
        fetch(url)
            .then((response) => response.json())
            .then((data) => this.$emit('searchResult', data, queryWasNull))
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.search-panel {
    width: 100%;

    input {
        width: 60%;
        margin: 0 auto;
    }

    button {
        margin-top: 2em;
    }
}
</style>