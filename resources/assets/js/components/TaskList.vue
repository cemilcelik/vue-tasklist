<template>
    <div>
        <h1>My Tasks</h1>
        <h3>New Task</h3>
        <form action="#" @submit.prevent="createTask()">
            <div class="input-group">
                <input type="text" class="form-control" v-model="task.body" name="body">
                <span class="input-group-btn">
                    <button type="submit" class="btn btn-primary">Add</button>
                </span>
            </div>
        </form>
        <h3>All Tasks</h3>
        <ul class="list-group">
            <li v-if="tasks.length === 0">There are no tasks yet!</li>
            <li class="list-group-item" v-for="(task, index) in tasks">
                {{ task.body }}
                <button class="btn btn-danger btn-xs pull-right" @click="deleteTask(task.id)">Delete</button>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data() {
        return {
            tasks: [],
            task: {
                id: '',
                body: ''
            }
        }
    },
    created() {
        this.fetchTaskList();
    },
    methods : {
        fetchTaskList() {
            axios.get('tasks').then((response) => {
                this.tasks = response.data;
            });
        },
        createTask () {
            axios.post('tasks', this.task).then((response) => {
                this.task.body = '';
                this.fetchTaskList();
            }).catch((error) => {
                console.log(error);
            });
        },
        deleteTask(id) {
            axios.delete('tasks/' + id).then((response) => {
                this.fetchTaskList();
            }).catch((error) => {
                console.log(error);
            });
        }
    }
}
</script>

<style>

</style>
