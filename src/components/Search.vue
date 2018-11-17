<template>
  <section class="search">
    <h1>Search by Github {{selectedSearchMethod | capitalize}}</h1>
    <form @submit.prevent="searchRepos">
      <input type="search" name="search" id="search" v-model="q" required>
      <label for="search">{{selectedSearchMethod | capitalize}} search</label>
      <SearchType @handleChange="$emit('handleChange', $event)" :selectedSearchMethod='selectedSearchMethod'/>
      <!-- Add component to template -->
      <!-- Continue to pass down props -->
    </form>
  </section>
</template>

<script>
// import component
import SearchType from '@/components/SearchType.vue';

export default {
  name: 'Search',
  components: {
    SearchType,
  },
  data() {
    return {
      q: '',
    };
  },
  props: {
    selectedSearchMethod: String
  },
  methods: {
    async searchRepos() {
      this.$emit('handleSearch', {
        key: 'isSearching',
        val: true,
      });
      this.$emit('resetSearch');
      const response = await fetch(this.searchEndpoint);
      
      // There's a different endpoint if the search method changes
      // Developer endpoint: https://api.github.com/users/${this.q}/repos

      const json = await response.json();

      const items = this.selectedSearchMethod === 'repo' ?
      json.items :
      json;
      // We will need to define object based on search type, since it returns a different data structure

      this.$emit('handleSearch', {
        key: 'isSearching',
        val: false,
      });

      if (items.length) {
        this.$emit('handleChange', {
          key: 'repos',
          val: items,
        });
      } else {
        this.$emit('handleSearch', {
          key: 'hasError',
          val: true,
        });
      }
    },
  },
  computed: {
    searchEndpoint(){
      return this.selectedSearchMethod === 'repo' ?
      `https://api.github.com/search/repositories?q=${this.q}` :
      `https://api.github.com/search/users?${this.q}/repos`
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass" scoped>

.search
  display: grid
  justify-content: center
  form
    position: relative
    label
      position: absolute
      left: 6px
      top: 10px
      color: var(--grey)
      background: white
      padding: 5px
      line-height: 1
      transition: 0.25s ease;
    input
      width: 100%
      box-sizing: border-box
      padding: 10px
      border-radius: 5px
      border: 2px solid var(--grey)
      &:hover,
      &:active,
      &:focus,
      &:valid
        &~ label
          top: -13px

</style>
