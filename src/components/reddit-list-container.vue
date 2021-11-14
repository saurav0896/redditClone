<template>
  <v-container>
    <v-layout row class="my-3">
        <v-flex v-for="item in searchData" :key="item.data.id"  class=" py-0">
          <redditBox :data="item.data"/>
        </v-flex>
    </v-layout>
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