<template>
  <div>
    <post-gallery :posts="posts" />
    <comment-gallery :comments="comments" />
    <div class="newComment" v-if="user">
      <form @submit.prevent="sendComment">
        <p>Add a comment:</p>
        <textarea v-model="comment" placeholder="comment"></textarea>
        <p></p>
        <button type="submit" class="pure-button pure-button-secondary">Submit</button>
      </form>
    </div>
  </div>
</template>


<script>
import PostGallery from '@/components/PostGallery.vue'
import CommentGallery from '@/components/CommentGallery.vue'

export default {
  name: 'PostPage',
  components: {
    PostGallery,
    CommentGallery,
  },
  data() {
    return {
      comment: '',
      error: '',
    }
  },
  props: {
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
    posts() {
      return this.$store.state.posts;
    },
    comments() {
      return this.$store.state.comments;
    },
  },
  async created() {
    await this.$store.dispatch("getUser");
    await this.$store.dispatch("getPost", this.$route.params.id);
    await this.$store.dispatch("getComments", this.$route.params.id);
  },
  methods: {
  async sendComment() {
    try {
      //const formData = new FormData();
      //formData.append('body', this.comment);
      //formData.append('photo', this.$route.params.id);
      this.error = await this.$store.dispatch("postComment", {body: this.comment, post: this.$route.params.id});
      if (!this.error) {
        this.comment = '';
        this.$emit('uploadFinished');
      }
    } catch (error) {
      console.log(error);
    }
    await this.$store.dispatch("getComments", this.$route.params.id);
  },
 },
}
</script>

<style scoped>

textarea {
  width: 100%;
  height: 100px
}

.pure-button-secondary {
  float: right;
}

.newComment {
  width: 50%;
  margin: auto;
  margin-top: 25px;
  text-align: left;
  padding: 10px;
  border-radius: 10px;
  background: rgba(230, 230, 230, 0.3);
}
</style>
