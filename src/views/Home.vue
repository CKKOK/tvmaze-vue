<template>
  <div>
    <div class="home">
      TVMaze-Vue!
    </div>
    <SearchPanel @searchResult="onReceiveSearch" v-if="view == 'search'"/>
    <ResultsPanel :results="results" :infiniteScroll="infiniteScroll" :viewChange="viewChange" v-else/>
  </div>
</template>

<script lang="ts">
  import { Component, Prop, Vue } from 'vue-property-decorator';
  import SearchPanel from '@/components/SearchPanel.vue';
  import ResultsPanel from '@/components/ResultsPanel.vue';

  @Component({
    components: {
      SearchPanel,
      ResultsPanel,
    },
  })

  export default class Home extends Vue {
    view: string = 'search';
    results: object[];
    infiniteScroll: boolean = false;

    mounted() {

    }
    
    viewChange(view: string) {
      this.view = view;
    }

    onReceiveSearch(data: object[], queryWasNull: boolean = false) {
      this.results = data;
      this.infiniteScroll = queryWasNull;
      this.viewChange('results');
    }
  }
</script>

<style scoped lang="scss">
.home {
  width:100%;
  font-size: 5em;
  font-weight: 600;
  padding-top: 1em;
  text-align: center;
  color:rgb(29, 141, 25);
}
</style>
