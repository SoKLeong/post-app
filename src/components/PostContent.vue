<template>
    <Tools @search="searching" :user_id="user_id" />
    <div v-show="posts.length > 0">
        <div class="d-flex mb-3">
            <span>Page {{ records.currentPage }} / {{ records.totalPage }}</span>
        </div>
        <Posts :posts="posts" @prev-post="prevPost" @next-post="nextPost" @toggle-edit="toggleEdit"
            @delete-post="deletePost" @jump-post="jumpPost" :user_id="user_id" :pages="pages" :records="records" />
    </div>
    <div id="noPost" v-show="posts.length <= 0">
        <img src="@/assets/nopost.png" alt="">
        <h4>Opps, no posts were found.</h4>
    </div>
    <AddPost @add-post="addPost" />
    <EditPost @edit-post="editPost" :postToEdit="postToEdit" />
</template>

<script>
import Tools from '@/components/Tools.vue';
import Posts from '@/components/Posts.vue';
import AddPost from '@/components/AddPost.vue';
import EditPost from '@/components/EditPost.vue';

export default {
    name: "PostContent",
    components: {
        Tools,
        Posts,
        AddPost,
        EditPost
    },
    props: {
        user_id: Number,
        url: String,
    },
    data() {
        return {
            posts: [],
            search: '',
            records: {
                currentPage: 1,
                totalPage: 100
            },
            pages: [],
            postToEdit: {}
        }
    },
    methods: {
        async fetchPosts() {
            return await fetch(`${this.url}?title=${this.search}&page=${this.records.currentPage}`, {
                headers: {
                    'Authorization': 'Bearer e32603fef138c73daea4e6fe78d735976da850a63065c41804a67b1a938e5bd3',
                }
            }).then(response => {
                if (response.ok) {
                    this.records.totalPage = response.headers.get('X-Pagination-Pages')
                    this.records.currentPage = response.headers.get('X-Pagination-Page')
                    return response.json()
                } else {
                    return Promise.reject('Error: ' + response.status)
                }
            }).catch(error => {
                return []
            })
        },
        async prevPost() {
            if (Number(this.records.currentPage) > 1) {
                this.records.currentPage--
                this.posts = await this.fetchPosts()
                window.scrollTo(0, 0)
                this.paging()
            }
        },
        async nextPost() {            
            if (Number(this.records.currentPage) < Number(this.records.totalPage)) {
                this.records.currentPage++
                this.posts = await this.fetchPosts()
                window.scrollTo(0, 0)
                this.paging()
            }
        },
        async jumpPost(page) {
            this.records.currentPage = page
            this.posts = await this.fetchPosts()
            window.scrollTo(0, 0)
            this.paging()
        },
        async searching(input) {
            this.search = input
            this.records.currentPage = 1
            this.posts = await this.fetchPosts()
            this.paging()
        },
        async addPost(post) {
            await fetch(`https://gorest.co.in/public/v2/users/${this.user_id}/posts`, {
                method: 'POST',
                headers: {
                    'Content-type': 'application/json',
                    'Authorization': 'Bearer e32603fef138c73daea4e6fe78d735976da850a63065c41804a67b1a938e5bd3',
                },
                body: JSON.stringify(post)
            }).then(response => {
                if (response.ok) {
                    return response.json()
                } else {
                    return Promise.reject('Error: ' + response.status)
                }
            }).then(data => {
                this.posts = [data, ...this.posts]
                alert('Post added successfully.')
                $('#addPostModal').modal('hide')
            }).catch(error => {
                alert('Failed to add post, ' + error)
            })

        },
        toggleEdit(post) {
            this.postToEdit = { ...post }
        },
        async editPost(editedPost) {
            await fetch(`https://gorest.co.in/public/v2/posts/${editedPost.id}`, {
                method: 'PUT',
                headers: {
                    'Content-type': 'application/json',
                    'Authorization': 'Bearer e32603fef138c73daea4e6fe78d735976da850a63065c41804a67b1a938e5bd3',
                },
                body: JSON.stringify(editedPost)
            }).then(response => {
                if (response.ok) {
                    return response.json()
                } else {
                    return Promise.reject('Error: ' + response.status)
                }
            }).then(data => {
                this.posts = this.posts.map((post) =>
                    post.id === data.id ? data : post
                )
                alert('Post edited successfully.')
                $('#editPostModal').modal('hide')
            }).catch(error => {
                alert('Failed to edit, ' + error)
            })
        },
        async deletePost(deletePost) {
            if (confirm(`Confirm delete ${deletePost.title}?`)) {
                await fetch(`https://gorest.co.in/public/v2/posts/${deletePost.id}`, {
                    method: 'DELETE',
                    headers: {
                        'Authorization': 'Bearer e32603fef138c73daea4e6fe78d735976da850a63065c41804a67b1a938e5bd3',
                    }
                }).then(response => {
                    if (response.ok) {
                        this.posts = this.posts.filter(post => post.id !== deletePost.id)
                        alert('Post delete successfully.')
                    } else {
                        return Promise.reject('Error: ' + response.status)
                    }
                }).catch(error => {
                    alert('Failed to delete, ' + error)
                })
            }
        },
        paging() {
            this.pages = []
            for (let i = this.records.currentPage - 3, j = 1; j <= 7 && i <= this.records.totalPage; i++) {
                if (i >= 1 && i) {
                    this.pages.push(i)
                    j++;
                }
            }
        },
    },
    async created() {
        this.posts = await this.fetchPosts()
        this.paging()
    }
}
</script>

<style lang="scss" scoped>
    #noPost {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        padding: 36px;

        img {
            width: 100%;
            height: 100%;
            object-fit: scale-down;
        }

        h4 {
            text-align: center;
        }

    }
</style>