<template>
  <div class="results-panel">
      <button class="btn btn-primary" @click="back">Back to Search</button>
      <div v-if="infiniteScroll == false" class="results-container">
          <SearchResult v-for="result in results" v-bind:key="result.show.name" v-bind:name="result.show.name" v-bind:url="result.show.url" v-bind:imgurl="result.show.image.medium" v-bind:summary="result.show.summary" />
      </div>
      <div v-else class="results-container">
          <SearchResult v-for="result in results" v-bind:key="result.name" v-bind:name="result.name" v-bind:url="result.url" v-bind:imgurl="result.image ? result.image.medium : ''" v-bind:summary="result.summary" />
      </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import SearchResult from '@/components/SearchResult.vue';
const urlRoot = 'http://api.tvmaze.com/shows?page=';
@Component({
    name: 'ResultsPanel',
    components: {
        SearchResult
    }
})

export default class ResultsPanel extends Vue {
    @Prop({default: []})    results: object[];
    @Prop({default: false}) infiniteScroll: boolean;
    @Prop()                 viewChange: (view: string) => void;

    fetching: boolean = false;
    page: number = 0;

    mounted () {
        if (this.infiniteScroll === true) {
            window.addEventListener('scroll', this.onScroll, false);
            this.page = 1;
        }
        this.onScroll = this.onScroll.bind(this);
    }

    beforeDestroy () {
        if (this.infiniteScroll === true) {
            window.removeEventListener('scroll', this.onScroll, false);
        }
    }

    back() {
        this.viewChange('search');
    }

    addResults(data: object[]) {
        this.results = this.results.concat(data);
        this.fetching = false;
        this.page++;
    }

    onScroll() {
        if (window.innerHeight + window.scrollY >= document.body.offsetHeight - 500 && !this.fetching) {
            this.fetching = true;
            fetch(urlRoot + (this.page + 1).toString())
                .then((response) => response.json())
                .then((data) => this.addResults(data));
        };
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.results-panel {
    width: 100%;
    text-align: center;
    padding-top: 2em;
}

.results-container {
    margin-top: 2em;
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    align-self: center;
}
</style>
