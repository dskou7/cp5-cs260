<template>
  <div class="home">
    <div class="header" v-if="user">
      <div>
        <p>{{user.name}} | <a href="#" @click="logout">Logout</a></p>
      </div>
    </div>
    <div class="header" v-else>
      <router-link to="/register" class="pure-button">Register</router-link> or
      <router-link to="/login" class="pure-button">Login</router-link>
    </div>
    <post-gallery :posts="posts" />
  </div>

</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'
import PostGallery from '@/components/PostGallery.vue'
export default {
  name: 'home',
  components: {
    PostGallery,
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
    posts() {
      return this.$store.state.posts;
    },
  },
  async created() {
    await this.$store.dispatch("getUser");
    await this.$store.dispatch("getAllPosts");
  },
  methods: {
    async logout() {
      try {
        this.error = await this.$store.dispatch("logout");
      } catch (error) {
        console.log(error);
      }
    },
  },
}
</script>

<style>
.header {
  float: right;
  padding-right: 20px;
}
</style>
