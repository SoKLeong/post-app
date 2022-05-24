<template>
    <div class="modal fade" id="addPostModal" tabindex="-1" role="dialog">
        <div class="modal-dialog modal-dialog-centered modal-lg" role="document">
            <div class="modal-content">
                <form @submit="addPost">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLongTitle">Add Post</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">

                        <div class="form-group">
                            <label for="postTitle">Post Title</label>
                            <input type="text" v-model="title" class="form-control" id="postTitle"
                                placeholder="Your post title">
                        </div>
                        <div class="form-group">
                            <label for="postBody">Post Body</label>
                            <textarea class="form-control" v-model="body" id="postBody" rows="3"
                                placeholder="Your post content"></textarea>
                        </div>

                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <button type="submit" class="btn btn-primary">Add Post</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AddPost',
    data() {
        return {
            title: '',
            body: ''
        }
    },
    emits: ['add-post'],
    methods: {
        addPost(e) {
            e.preventDefault()

            if (!this.title) {
                alert('Please enter a title.')
                return
            }

            if (!this.body) {
                alert('Please enter something in the post body.')
                return
            }

            const newPost = {
                title: this.title,
                body: this.body
            }

            this.$emit('add-post', newPost)

            this.title = ''
            this.body = ''

        }
    }
}

</script>