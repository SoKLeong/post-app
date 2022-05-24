<template>
    <div :class="[todo.status == 'completed' ? 'complete' : '', 'todo']" @dblclick="$emit('toggle-status', todo)">
        <div class="mr-3">
            <p>{{ todo.title }}</p>
            <span class="d-block mb-2 text-primary">{{ todo.due_on ? daysLeft : ''}} </span>
            <span class="due">Due: {{ todo.due_on ? formatDate : 'No Due' }}</span>
        </div>
        <button @click="$emit('delete-task', todo)" class="btn btn-white text-danger"><i class="fa fa-close"></i></button>
    </div>
</template>

<script>
export default {
    name: 'ToDo',
    props: {
        todo: Object
    },
    emits: ['toggle-status', 'delete-task'],
    methods: {
    },
    computed: {
        formatDate() {
            if (this.todo.due_on) {
                const date = new Date(this.todo.due_on)
                return date.toLocaleDateString("en-SG", {
                    year: 'numeric',
                    month: 'long',
                    day: 'numeric'
                }) + ' ' + date.toLocaleTimeString("en-SG", {
                    hour: 'numeric',
                    minute: 'numeric',
                    timeZone: 'Asia/Calcutta'
                })
            }
        },
        daysLeft() {
            const oneDay = 24 * 60 * 60 * 1000; // hours*minutes*seconds*milliseconds
            const firstDate = new Date();
            const secondDate = new Date(this.todo.due_on);

            const days = Math.round((secondDate - firstDate) / oneDay);
            console.log(days)
            if(days < 0) {
                return 'Overdue'
            } else {
                return days + ' days left'
            }
        }
    }
}

</script>

<style lang="scss" scoped>
@import "@/scss/_variables.scss";

.todo {
    display: flex;
    justify-content: space-between;
    background-color: #f8f9fa;
    padding: 12px;
    margin-bottom: 8px;
    border-left: 5px solid red;
    transition: background-color 0.1s;

    &.complete {
        border-left: 5px solid green;
    }

    &:hover {
        cursor: pointer;
        background-color: darken(#f8f9fa, 5%);
    }

    p {
        margin-bottom: 0px;
        font-weight: bold;
    }

    span {
        font-size: 14px;
        color: #333;

        &.due {
            background-color: darken(#f8f9fa, 5%);
            padding: 2px;
        }
    }

    button:hover {
        background-color: lighten(red, 40%)
    }
}
</style>