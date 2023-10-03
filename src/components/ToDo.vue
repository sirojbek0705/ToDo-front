<template>
    <div class="container">
        <div class="container-fluid">
            <div class="search">
                <input v-model="projectName" @input="saveProjectName">
            </div>
            <div class="task">
                <h1>Tasks</h1>
            </div>
            <div>
                <input v-model="newTask" @keyup.enter="addTask" placeholder="Add new task">
            </div>
            <div class="item-list">
                <ul>
                    <li v-for="(task, index) in tasks" :key="index">
                        <input type="checkbox" v-model="task.done">
                        <span :class="{done: task.done}" v-if="!task.editing">{{ task.text }}</span>
                        <input v-model="task.text" v-else @blur="saveEditedTask(task)" @keyup.enter="saveEditedTask(task)" />
                        <button @click="editTask(task)">{{ task.editing ? 'Save' : 'Edit' }}</button>
                        <button @click="deleteTask(index)">Delete</button>
                        <hr>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'ToDo',
    data() {
        return {
            projectName: '',
            newTask: '',
            tasks: []
        }
    },
    methods: {
        saveProjectName() {
            localStorage.setItem('projectName', this.projectName);
        },
        addTask() {
            if (this.newTask.trim() === '') return;
            this.tasks.push({ text: this.newTask, done: false });
            this.newTask = '';
            this.saveTasks();
        },
        deleteTask(index) {
            this.tasks.splice(index, 1);
            this.saveTasks();
        },
        editTask(task) {
            task.editing = !task.editing;
            this.saveTasks();
        },

        saveEditedTask(task) {
            task.editing = false;
            this.saveTasks();
        },
        saveTasks() {
            localStorage.setItem('tasks', JSON.stringify(this.tasks));
        }
    },
    created() {
        this.projectName = localStorage.getItem('projectName') || '';
        this.tasks = JSON.parse(localStorage.getItem('tasks')) || [];
    }
}
</script>

<style>
body {
    margin: 0;
    font-family: sans-serif;
}

input {
    border-radius: 5px;
    border: none;
}


.container {
    width: 1140px;
    margin:  0 auto;
    display: flex;
    justify-content: center;
    flex-direction: row;
    background-color: #d9d9d9;
    box-sizing: border-box;
}
.item-list {
    width: 400px;
}
li {
    list-style-type: none;
}
.search {
    margin-top: 2rem;
}

button {
    margin-top: 4px;
    margin-left: 1rem;
    border-radius: 5px;
    border: none;
}
</style>
