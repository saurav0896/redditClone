<template>
  <v-app>
    <v-app-bar app>
      <div class="d-flex align-center">
        <v-img
          alt="Reddit Logo"
          class="shrink"
          contain
          src="https://logodownload.org/wp-content/uploads/2018/02/reddit-logo.png"
          transition="scale-transition"
          width="140"
        />
      </div>

      <v-spacer></v-spacer>
      <template v-if="searchResult.length > 0">
        <v-text-field
          class="mt-6"
          color="#FF4500"
          v-model="searchData"
          dense
          solo
          flat
          outlined
          label="Search Reddit"
          append-icon="mdi-magnify"
        ></v-text-field>

        <v-select
          :items="['relevance', 'hot', 'top', 'new', 'comments']"
          class="mt-6 ml-3 select"
          dense
          background-color="white"
          v-model="sortBy"
          label="Sort By"
          color="#FF4500"
          solo
          outlined
        ></v-select>
        <v-btn
          color="#FF4500"
          dark
          class="mx-2 py-5"
          :loading="loader"
          @click="getSearch(searchData, sortBy)"
        >
          Search
        </v-btn>
      </template>
    </v-app-bar>

    <v-main>
      <template v-if="searchResult.length == 0">
        <v-container>
          <v-sheet style="width: 50%" class="mx-auto mt-9 pa-6 elevation-1">
            <v-text-field
              class="mt-6"
              color="#FF4500"
              v-model="searchData"
              dense
              solo
              flat
              outlined
              label="Search Reddit"
              append-icon="mdi-magnify"
            ></v-text-field>
            <!-- <v-select
              :items="['relevance', 'hot', 'top', 'new', 'comments']"
              class="mt-6 ml-3"
              dense
              background-color="white"
              v-model="sortBy"
              label="Sort By"
              color="#FF4500"
              solo
              outlined
            ></v-select> -->
            <label>Sort By : </label>
            <v-radio-group  v-model="sortBy" row>
              <v-radio color="#FF4500" label="Relevance" value="relevance"></v-radio>
              <v-radio color="#FF4500" label="Hot" value="hot"></v-radio>
              <v-radio color="#FF4500" label="Top" value="top"></v-radio>
              <v-radio color="#FF4500" label="New" value="new"></v-radio>
              <v-radio color="#FF4500" label="Comments" value="comments"></v-radio>
            </v-radio-group>
            <v-btn
              color="#FF4500"
              dark
              class="mx-2 py-5"
              @click="getSearch(searchData, sortBy)"
              :loading="loader"
            >
              Search
            </v-btn>
          </v-sheet>
        </v-container>
      </template>
      <template v-else>
           <redditContainer :result="searchResult" :key="refreshComponent" />
      </template>
      <template v-if="postLoading">
        <div class="text-center">
          <v-progress-circular
            indeterminate
            color="#FF4500"
          ></v-progress-circular>
        </div>
      </template>
    </v-main>
  </v-app>
</template>

<script>
import redditContainer from "./components/reddit-list-container";
import axios from "axios";

export default {
  name: "App",

  components: {
    redditContainer,
  },

  data: () => ({
    //
    searchData: null,
    loader: false,
    sortBy: "relevance",
    refreshComponent: 0,
    searchResult: [],
    postLoading: false
  }),
  
  methods: {
    getSearch(data, sort) {
      
      this.loader = true;
      axios
        .get(`https://www.reddit.com/search.json?q=${data}&sort=${sort}`)
        .then((res) => {
          this.searchResult = res.data.data.children;
          this.loader = false;
          document.body.scrollTop = 0;
          document.documentElement.scrollTop = 0;
          this.refreshComponent++;
        })
        .catch((error) => {
          this.loader = false;
          console.log("Error:", error);
        });
    },
    getDataOnScroll(){
      var scrollData = false;
      //Checking if reached Bottom or not using Browerser Properties
      window.addEventListener("scroll", () => {
        const { scrollTop, scrollHeight, clientHeight } = document.documentElement;

        

        if (clientHeight + scrollTop >= scrollHeight - 5) {

           if(!scrollData){
            scrollData = true;
            this.postLoading = true;
            axios
              .get(`https://www.reddit.com/search.json?q=${this.searchData}&sort=${this.sortBy}`)
              .then((res) => {
                let resData = res.data.data.children;

                resData.forEach(element => {
                  this.searchResult.push(element);
                });
                this.postLoading = false;
                this.refreshComponent++;
                scrollData = false;
              })
              .catch((error) => {
                console.log("Error:", error);
                this.postLoading = false;
              });
           }
          
        }
      })
    }
  },
  mounted(){
    this.getDataOnScroll();
  }
};
</script>
<style scoped>
.select {
  width: 30px;
}
</style>
