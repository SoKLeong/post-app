<template>
    <div class="modal fade" id="addTaskModal" tabindex="-1" role="dialog">
        <div class="modal-dialog modal-dialog-centered modal-lg" role="document">
            <div class="modal-content">
                <form @submit="addPost">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLongTitle">Add Task</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">

                        <div class="form-group">
                            <label for="taskTitle">Task Title</label>
                            <input type="text" v-model="title" class="form-control" id="taskTitle"
                                placeholder="Your post title">
                        </div>
                        <div class="form-group">
                            <label for="taskDue">Due Date</label>
                            <input class="form-control" type="datetime-local" v-model="due_on" name="" id="taskDue">
                        </div>

                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <button type="submit" class="btn btn-primary">Add Task</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AddTask',
    data() {
        return {
            title: '',
            due_on: ''
        }
    },
    emits: ['add-task'],
    methods: {
        addPost(e) {
            e.preventDefault()

            if (!this.title) {
                alert('Please enter a title.')
                return
            }

            if(this.due_on && Date.parse(this.due_on) <= Date.now()) {
                alert('Please select a date in the future.')
                return
            }

            const newTask = {
                title: this.title,
                due_on: this.due_on,
                status: 'pending'
            }

            this.$emit('add-task', newTask)

            this.title = ''
            this.due_on = ''

        }
    }
}

</script>