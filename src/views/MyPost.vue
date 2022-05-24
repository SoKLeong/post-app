<template>
    <h4 class="mb-3 text-center d-block d-sm-none">My Post</h4>
    <PostContent :url="url" :user_id="user_id" />
</template>

<script>
import PostContent from "../components/PostContent.vue";
export default {
    name: "MyPost",
    components: { PostContent },
    data() {
        return {
            url: `https://gorest.co.in/public/v2/users/${this.user_id}/posts`
        }
    },
    props: {
        user_id: Number
    }
}

</script>

<style lang="scss" scoped>
</style>