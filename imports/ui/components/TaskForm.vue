<template>
    <form class="new-task" @submit.prevent="handleSubmit">
        <input class="task-input" type="text" placeholder="Type to add new tasks" v-model="newTask" />
        <button class="task-add-button" type="submit">Add Task</button>
    </form>
</template>

<script>
import {Meteor} from 'meteor/meteor';
import { TasksCollection } from "../../api/TasksCollection";

export default {
    data() {
        return {
            newTask: ""
        }
    },
    methods: {
        handleSubmit(event) {
            if(this.newTask.length === 0) return;

            Meteor.call('tasks.insert',this.newTask.trim());

            this.newTask = "";


        }
    },
}
</script>