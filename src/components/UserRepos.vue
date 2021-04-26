<template>
  <div class="container repos">
    <div v-for="repo in repos" :key="repo" class="card">
      <div class="card-body">
        <h6 class="text-left"><a :href="repo.html_url">{{repo.name}}</a> <span class="badge badge-pill badge-warning" v-if="repo.fork">Fork</span></h6>
        <p class="text-left" v-if="repo.description">{{repo.description}}</p>
        <span class="d-flex flex-row gap-1">
          <span><i class="fas fa-star"></i> {{repo.stargazers_count}} stars</span>
          <span><i class="fas fa-calendar"></i>Updated {{moment(repo.updated_at).fromNow()}} </span>
        </span>
      </div>
    </div>
  </div>
</template>

<script>

import axios from "axios";
//eslint-disable-next-line no-unused-vars
import moment from 'moment';

const api = axios.create({
  baseURL: "https://api.github.com/",
  headers: {
    Accept: "application/vnd.github.v3+json",
  },
});

const getPaginationLinks = function (data) {
  let arrData = data.split("link:")
  data = arrData.length == 2? arrData[1]: data;
  let parsed_data = {}

  arrData = data.split(",")

  for (let d of arrData){
    let linkInfo = /<([^>]+)>;\s+rel="([^"]+)"/ig.exec(d)

    parsed_data[linkInfo[2]]=linkInfo[1]
  }

  return parsed_data;
}

export default {
  data() {
    return {
      repos: [],
      links: null,
      
    }
  },
  props: {
    username: String
  },
  beforeCreate() {
    this.moment = moment
  },
  created() {
    api
      .get('/users/' + this.username + '/repos')
      .then((response) => {
        this.repos = response.data;
        console.log(this.repos[0])
        this.links = getPaginationLinks(response.headers.link);
      });
  }

}
</script>

<style scoped>
.repos {
  height: 100%;
  overflow: scroll;
}
</style>