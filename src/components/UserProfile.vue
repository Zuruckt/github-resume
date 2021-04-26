<template>
  <div class="d-flex flex-column nowrap justify-content-start align-items-center m-4 gap-1">
    <img :src="user.avatar_url" alt="User profile photo" />
    <span>
      {{ user.name }}
    </span>
    <span class="font-weight-light"> @{{ username }} </span>
    <p class="lead my-1" v-if="user.bio">
        <small>
            {{user.bio}}
        </small>        
    </p>
    <div class="icons d-flex flex-column align-items-start align-self-start">
        <span><i class="fas fa-users"></i> {{user.followers}} followers </span>
        <span><i class="far fa-star"></i>  {{user.following}} following</span>
        <span><i class="fas fa-star"></i> {{user.stars}} stars</span>
    </div>
    <div class="icons d-flex flex-column align-items-start align-self-start mt-2">
        <span v-if="user.company"><i class="fas fa-building"></i> {{user.company}}</span>
        <span v-if="user.location"><i class="fas fa-map-marker-alt"></i> {{user.location}}</span>
        <span v-if="user.email"><i class="fas fa-email"></i> {{user.email}}</span>
        <span v-if="user.blog"><i class="fas fa-link"></i>  <a :href="user.blog">{{user.blog}}</a></span>
        <span v-if="user.twitter_username"><i class="fas fa-twitter"></i> <a :href="'http://twitter.com/' + user.twitter_username">{{'http://twitter.com/' + user.twitter_username}}</a></span>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const api = axios.create({
  baseURL: "https://api.github.com/",
  headers: {
    Accept: "application/vnd.github.v3+json",
  },
});

export default {
  data() {
    return {
      user: {
        name: 'A Github User',
        avatar_url: '@/assets/user.png',
        followers: 0,
        following: 0,
        bio: null,
        email: null,
        location: null,
        company: null,
        blog: null,
        twitter_url: null,
        stars: 0
      },
    };
  },
  props: {
    username: String,
  },
  created() {
    api
      .get("users/" + this.username)
      .then((response) => {
        let data = response.data;
        console.log(data);
        this.user.name = data.name;
        this.user.bio = data.bio;
        this.user.avatar_url = data.avatar_url;
        this.user.followers = data.followers;
        this.user.following = data.following;
        this.user.company = data.company;
        this.user.location = data.location;
        this.user.email = data.email;
        this.user.blog = data.blog;
        this.user.twitter_username = data.twitter_username;
      })
      .catch((exception) => {
        console.log(exception);
      });
  },
};
</script>

<style scoped>
* {
  font-family: Lato;
}
img {
  width: 100px;
}
.lead {
    font-size: 11px;
    text-align: start;
}
.icons {
    font-size: 10px;
}
</style>