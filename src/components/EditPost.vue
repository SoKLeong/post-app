<template>
    <div class="modal fade" id="editPostModal" tabindex="-1" role="dialog">
        <div class="modal-dialog modal-dialog-centered modal-lg" role="document">
            <div class="modal-content">
                <form @submit="editPost">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLongTitle">Edit Post</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">

                        <div class="form-group">
                            <label for="postTitle">Post Title</label>
                            <input type="text" v-model="postToEdit.title" class="form-control" id="postTitle"
                                placeholder="Your post title">
                        </div>
                        <div class="form-group">
                            <label for="postBody">Post Body</label>
                            <textarea class="form-control" v-model="postToEdit.body" id="postBody" rows="3"
                                placeholder="Your post content"></textarea>
                        </div>

                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <button type="submit" class="btn btn-primary">Edit Post</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'EditPost',
    props: {
        postToEdit: Object
    },
    emits: ['edit-post'],
    methods: {
        editPost(e) {
            e.preventDefault()
            if (!this.postToEdit.title) {
                alert('Please enter a title.')
                return
            }

            if (!this.postToEdit.body) {
                alert('Please enter something in the post body.')
                return
            }

            const editedPost = this.postToEdit

            this.$emit('edit-post', editedPost)
        }
    }
}

</script>