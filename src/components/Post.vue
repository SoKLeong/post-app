<template>
    <div class="post">
        <div class="pTitle">{{ post.title }}</div>
        <hr>
        <div class="pBody mb-2">{{ post.body }}</div>
        <div v-show="this.user_id == post.user_id">
            <a href="#" @click="toggleEdit" class="mr-2">Edit</a>
            <a href="#" @click="$emit('delete-post', post)" class="text-danger">Delete</a>
        </div>
    </div>
</template>

<script>
export default {
    name: "Post",
    props: {
        post: Object,
        user_id: Number
    },
    emits: ['toggle-edit', 'delete-post'],
    data() {
        return {
            username: ''
        }
    },
    methods: {
        toggleEdit() {
            $("#editPostModal").modal("show")
            this.$emit('toggle-edit', this.post)
        }
    },

}
</script>

<style lang="scss" scoped>
@import "@/scss/_variables.scss";

.post {
    background-color: white;
    padding: 24px;
    margin-bottom: 16px;
    box-shadow: 2px 1px 5px #0000000f;
    transition: 0.2s;

    &:hover {
        transform: scale(1.01);
    }
}

.pTitle {
    padding: 16px;
    background-color: $primaryColor;
    color: white;
    font-weight: bold;
}

hr {
    font-size: 20px;
    border: 2px solid black;
    width: 25%;
    margin-left: 0;
}

a {

}
</style>