<template>
    <div :class="[todoView ? 'show' : '', 'col-0 col-xl-3 p-4 todoList']">
        <div class="d-flex justify-content-between">
            <h6>To Do List</h6>
            <button v-show="user_id ? true : false" class="btn btn-primary" data-toggle="modal"
                data-target="#addTaskModal"><i class="fa fa-add"></i>
                Add Task</button>
        </div>
        <hr>
        <span class="tips">{{ user_id ? 'Double click task to mark as done / undone' : 'Login to add some tasks 😊'
        }}</span>
        <div :key="todo.id" v-for="todo in todos">
            <ToDo :todo="todo" @toggle-status="toggleStatus" @delete-task="deleteTask" />
        </div>
        <div id="noTask" v-show="todos.length <= 0 && user_id">
            <img src="@/assets/notask.png" alt="">
            <h4>Opps, no task to show</h4>
            <span>Start your day by adding some tasks!😊</span>
        </div>
    </div>
    <AddTask @add-task="addTask" />
</template>

<script>
import ToDo from './ToDo.vue';
import AddTask from './AddTask.vue';
export default {
    name: "TodoList",
    components: { ToDo, AddTask },
    data() {
        return {
            todos: []
        }
    },
    props: {
        user_id: Number,
        todoView: Boolean
    },
    methods: {
        async fetchTodos() {
            await fetch(`https://gorest.co.in/public/v2/users/${this.user_id}/todos`, {
                headers: {
                    'Authorization': 'Bearer e32603fef138c73daea4e6fe78d735976da850a63065c41804a67b1a938e5bd3'
                }
            }).then(response => {
                if (response.ok) {
                    return response.json()
                }
                else {
                    return Promise.reject('Error: ' + response.status)
                }
            }).then(data => {
                this.todos = data
            }).catch(error => {
                alert(`Failed to retrieve todo list, ${error}`)
            })
        },
        async addTask(newTask) {
            await fetch(`https://gorest.co.in/public/v2/users/${this.user_id}/todos`, {
                method: 'POST',
                headers: {
                    'Content-type': 'application/json',
                    'Authorization': 'Bearer e32603fef138c73daea4e6fe78d735976da850a63065c41804a67b1a938e5bd3',
                },
                body: JSON.stringify(newTask)
            }).then(response => {
                if (response.ok) {
                    return response.json()
                } else {
                    return Promise.reject('Error: ' + response.status)
                }
            }).then(data => {
                this.todos = [data, ...this.todos]
                alert('Task added successfully.')
                $('#addTaskModal').modal('hide')
            }).catch(error => {
                alert('Failed to add task, ' + error)
            })
        },
        async toggleStatus(updateTodo) {
            const newStatus = updateTodo.status == 'pending' ? 'completed' : 'pending'
            await fetch(`https://gorest.co.in/public/v2/todos/${updateTodo.id}`, {
                method: 'PUT',
                headers: {
                    'Content-type': 'application/json',
                    'Authorization': 'Bearer e32603fef138c73daea4e6fe78d735976da850a63065c41804a67b1a938e5bd3',
                },
                body: JSON.stringify({ ...updateTodo, status: newStatus })
            }).then(response => {
                if (response.ok) {
                    return response.json()
                } else {
                    return Promise.reject('Error: ' + response.status)
                }
            }).then(data => {
                this.todos = this.todos.map(updatedTodo =>
                    updatedTodo.id == data.id ? data : updatedTodo
                )
            }).catch(error => {
                alert('Failed to add task, ' + error)
            })
        },
        async deleteTask(deleteTask) {
            if (confirm(`Confirm delete ${deleteTask.title}?`)) {
                await fetch(`https://gorest.co.in/public/v2/todos/${deleteTask.id}`, {
                    method: 'DELETE',
                    headers: {
                        'Authorization': 'Bearer e32603fef138c73daea4e6fe78d735976da850a63065c41804a67b1a938e5bd3',
                    }
                }).then(response => {
                    if (response.ok) {
                        this.todos = this.todos.filter(todo => todo.id !== deleteTask.id)
                    } else {
                        return Promise.reject('Error: ' + response.status)
                    }
                }).catch(error => {
                    alert('Failed to delete, ' + error)
                })
            }
        },
    },
    watch: {
        user_id: function (val) {
            this.fetchTodos()
        }
    }
}
</script>

<style lang="scss" scoped>
@import "@/scss/_variables.scss";

h6 {
    text-transform: uppercase;
    display: block;
    padding: 8px;
    font-weight: bold;
    margin-bottom: 0px;
}

.todoList {
    background-color: white;
    height: calc(100vh - 70px);
    padding: 8px;
    position: fixed;
    right: 0;
    z-index: 3;
    overflow-y: auto;
}

hr {
    border: 1px solid black;
}

button {
    font-size: 14px;
}

.tips {
    font-size: 14px;
    text-align: center;
    display: block;
    color: #555;
    background-color: #e9ecea;
    padding: 4px;
    margin-bottom: 16px;
}

#noTask {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 24px;
    width: 100%;

    img {
        width: 100%;
        height: 100%;
        object-fit: scale-down;
        padding: 16px;
    }

    h4 {
        text-align: center;
        margin-bottom: 16px;
    }

    span {
        font-size: 14px;
        text-align: center;
        padding: 4px;
        background-color: #e9ecea;
    }
}

@media only screen and (max-width: 1200px) {
    .todoList {
        width: 100%;
        left: 100%;
        top: 70px;
        transition: 0.4s;
        padding: 8px;

        &.show {
            left: 0%;
        }
    }
}
</style>