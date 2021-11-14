<template>
  <v-card  class="mx-auto my-2" width="350">
        <v-img
          height="250"
          :src="data.thumbnail.match(/http/i)  ? data.thumbnail : 'https://i.redd.it/yksc27b8noiz.png'"
        ></v-img>

        <v-card-title @click="searchNew(data.url)" class="selected">{{ truncateString(data.title, 80)}}</v-card-title>

        <v-card-text>
          <div class="my-4 text-subtitle-1"><strong>Posted By : </strong> {{ data.subreddit}}</div>

          <div>
            {{ data.selftext != '' ? truncateString(data.selftext, 200) : ""}}
          </div>
        </v-card-text>

        <v-card-actions>
          <v-btn color="#FF4500" @click="searchNew(data.url)" text> Read More </v-btn>
        </v-card-actions>
      </v-card>
</template>

<script>
export default {
    props:['data'],
    methods: {
        truncateString(string, limit){
            if (string.length < limit) return string;
            else {
              return string.substring(0,limit) + " ..."
            }
        },
        searchNew(url){
            window.open(url,'_blank');
        }
    }

}
</script>

<style>
.selected:hover{
    text-decoration: underline;
    cursor: pointer;
    color: #FF4500
}
</style>