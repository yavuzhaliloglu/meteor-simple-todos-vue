<template>
    <li v-bind:class="taskClassName">
        <input class="checkbox" type="checkbox" readonly v-bind:checked="!!this.task.isChecked" @click="toggleChecked">
        <span class="text">{{ this.task.text }}</span>
        <button class="delete-task-button" @click="deleteTask"> Delete Task </button>
    </li>
</template>

<script>
import {Meteor} from 'meteor/meteor'
import { TasksCollection } from '../../api/TasksCollection';

export default {
    props: ['task'],
    data() {
        return {};
    },
    computed:{
        taskClassName: function(){
        return this.task.isChecked ? "checked" : "";
        }
    },
    methods:{
        toggleChecked(){
            // SET THE CHECKED PROPERTY TO THE OPPOSİTE OF İTS CURRENT VALUE
            Meteor.call('tasks.setIsChecked',this.task._id, !this.task.isChecked)
        },
        deleteTask(){
            Meteor.call('tasks.remove',this.task._id)
        }
    }
};  
</script>

<style>

</style>