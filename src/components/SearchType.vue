<template>
  <div>
    <h2>Search Type</h2>
    <label v-for="(type, i) in searchMethods" :key="i" :for="type">
      {{type}}
      <input type="radio" name="selectedSearchMethod" :id="type" :value="type" :checked="selectedSearchMethod === type" @change="updateSearchType($event)">
    </label>
  </div>
</template>

<script>
export default {
  name: 'SearchType',
  methods: {
    updateSearchType(event){
      const {name, value} = event.target;
      this.$emit('handleChange', {
        key: name,
        val: value
      });
    },
  },
  props:{
    selectedSearchMethod: String,
  },
  data(){
    return{
      searchMethods: ['repo','developer']
    };
  }
};
</script>


<style lang="sass" scoped>
  h2
    font-size: 2rem
    margin-bottom: 0
  label
    margin: 0 10px
</style>

