<template>
  <v-container>
    <div class="d-flex" style="flex-wrap:wrap">
    <div v-for="item in searchData" :key="item.data.id"  class="mx-4 my-0 py-0" style="height: max-content">
       <redditBox :data="item.data"/>
    </div>
    </div>
  </v-container>
</template>

<script>
import redditBox from "./reddit-list-item"
export default {
  components: {
      redditBox
  },
  props: ["result"],
  data() {
    return {
      searchData: null,
    };
  },
  mounted() {
    this.searchData = this.result;
    console.log(this.searchData.length);
    console.log(this.searchData);
    
  },
};
</script>

<style>
</style>