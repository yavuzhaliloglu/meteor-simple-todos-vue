<template>

  <div class="app">

    <header>
      <div class="app-bar">
        <div class="app-header">
          <h1>To Do List</h1>
          <span v-if="incompleteCount > 0">({{ incompleteCount }})</span>
        </div>
      </div>
    </header>

    <div class="main">
      <template v-if="currentUser">
        <div class="user" v-on:click="logout">
          Merhaba, {{ currentUser.username }}  <span>Çıkış Yap🚪</span>
        </div>

        <div class="taskform">
          <TaskForm />
        </div>

        <div class="filter">
          <button v-bind="hideCompleted" @click="toggleHideCompleted">
            <span v-if="hideCompleted">Show All</span>
            <span v-else>Hide Completed Tasks</span>
          </button>
        </div>

        <div class="loading" v-if="!$subReady.tasks">Loading...</div>

        <ul class="tasks">
          <Task class="task" v-for="task in tasks" v-bind:key="task._id" v-bind:task="task" />
        </ul>
      </template>

      <template v-else>
        <LoginForm />
      </template>
    </div>
  </div>

</template>

<script>
import Vue from 'vue';
import Task from './components/Task.vue';
import TaskForm from './components/TaskForm.vue';
import { TasksCollection } from '../api/TasksCollection';
import TaskForm1 from './components/TaskForm.vue';
import LoginForm from './components/LoginForm.vue';


export default {
  components: {
    Task,
    TaskForm,
    TaskForm1,
    LoginForm
  },
  data() {
    return {
      hideCompleted: false
    }
  },
  methods: {
    toggleHideCompleted() {
      this.hideCompleted = !this.hideCompleted;
    },
    logout() {
      Meteor.logout()
    }
  },
  meteor: {
    $subscribe: {
      'tasks': []
    },

    tasks() {
      if (!this.currentUser) {
        return [];
      }

      const hideCompletedFilter = { isChecked: { $ne: true } };

      const userFilter = this.currentUser ? { userId: this.currentUser._id } : {};

      const pendingOnlyFilter = { ...hideCompletedFilter, ...userFilter };


      console.log(TasksCollection.find({}).fetch())
      return TasksCollection.find(
        this.hideCompleted ? pendingOnlyFilter : userFilter,
        {
          sort: { createdAt: -1 },
        }
      ).fetch()

    },

    incompleteCount() {
      return TasksCollection.find({ isChecked: { $ne: true }, userId: this.currentUser._id }).count();
    },

    currentUser() {
      return Meteor.user();
    }
  },
};
</script>

<style>
body {
  font-family: sans-serif;
  padding: 10px;
}
</style>
