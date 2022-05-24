<template>
    <div class="input-group mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="basic-addon1"><i class="fa fa-search"></i></span>
        </div>
        <input type="text" class="form-control" placeholder="Search title..." v-model="searchInput" v-on:input="$emit('search', this.searchInput)"  >
        <button v-show="user_id ? true : false" class="btn btn-primary ml-2" style="font-size: 14px" data-toggle="modal" data-target="#addPostModal">
            <i class="fa fa-pencil"></i> Add Post
        </button>
    </div>

</template>

<script>
export default {
    name: 'Tools',
    emits: ['search'],
    props: {
        user_id: Number
    },
    data() {
        return {
            searchInput: ''
        }
    },
}
</script>