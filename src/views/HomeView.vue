<template>
  <h4 class="mb-3 text-center d-block d-sm-none">Home</h4>
  <PostContent url="https://gorest.co.in/public/v2/posts" :user_id="user_id" />
</template>

<script>
import PostContent from "../components/PostContent.vue";
export default {
    name: "HomeView",
    components: { PostContent },
    props: {
      user_id: Number
    }
}

</script>

<style lang="scss" scoped>

</style>