<template>
  <div class="d-flex flex-column justify-content-center align-items-center vh-100">
    <h1>Search Devs</h1>
    <div class="form-group d-flex flex-row gap-1">
        <input type="text" class="form-control" placeholder="Type the username here" v-model="username">
        <button @click="search" class="btn-sm btn btn-primary"><i class="fas fa-search"></i>Search</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
        username: null,
    }
  },
  methods: {
      search() {
          this.$router.push({name: 'UserProfile', params: {username: this.username}});
      }
  }
};
</script>

<style scoped>
h1 {
  text-transform: none;
  font-family: Lato, sans-serif;
  font-weight: 375;
  font-style: italic;
}
</style>
