<template>
  <div class="row">
    <div class="col-md-3 bg-dark text-white p-0 vh-100">
        <UserProfile :username="username"></UserProfile>
    </div>
    <div class="col-md-9 bg-light p-0 vh-100">
        <UserRepos></UserRepos>
    </div>
  </div>
</template>

<script>
import UserProfile from '/src/components/UserProfile';
import UserRepos from '/src/components/UserRepos';
export default {
  data() {
    return {
      username: this.$route.params.username,
    };
  },
  components: {
      UserProfile,
      UserRepos
  }
};
</script>