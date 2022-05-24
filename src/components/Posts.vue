<template>
    <div :key="post.id" v-for="post in posts">
        <Post :post="post" :user_id="user_id" @toggle-edit="$emit('toggle-edit', post)"
            @delete-post="$emit('delete-post', post)" />
    </div>
    <nav aria-label="Page navigation example">
        <ul class="pagination">
            <li class="page-item"><button @click="$emit('jump-post', 1)" class="page-link">First</button></li>
            <li class="page-item"><button @click="$emit('prev-post')" class="page-link">◀</button></li>
            <li :class="[records.currentPage == page ? 'active' : '', 'page-item d-none d-md-block']"
                v-for="page in pages" :key="page"><button @click="$emit('jump-post', page)" class="page-link">{{ page
                }}</button></li>
            <li class="page-item active d-block d-md-none"><button @click="$emit('jump-post', records.currentPage)"
                    class="page-link">{{ records.currentPage }}</button>
            </li>
            <li class="page-item"><button @click="$emit('next-post')" class="page-link">▶</button></li>
            <li class="page-item"><button @click="$emit('jump-post', records.totalPage)" class="page-link">Last</button>
            </li>
        </ul>
    </nav>
</template>

<script>
import Post from './Post.vue';
export default {
    name: "Posts",
    emits: ['prev-post', 'next-post', 'toggle-edit', 'delete-post', 'jump-post'],
    props: {
        posts: Array,
        user_id: Number,
        pages: Array,
        records: Object
    },
    components: {
        Post
    },
}
</script>

<style scoped lang="scss">
</style>