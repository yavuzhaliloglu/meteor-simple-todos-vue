<template>
    <form class="login-form" @submit.prevent="handleSubmit">
        <div class="login-container">
            <div>
                <label for="username">Username</label>
                <input type="text" id="username" name="username" placeholder="Username" required v-model="username">
            </div>

            <div>
                <label for="password">Password</label>
                <input type="password" id="password" name="password" placeholder="Password" required v-model="password">
            </div>

            <div>
                <button class="login-button" type="submit">Log In</button>
            </div>
        </div>
    </form>
</template>



<script>
import {Meteor} from 'meteor/meteor';

export default{
    name: 'LoginForm',
    data(){
        return{
            username:"",
            password:""
        }
    },
    methods:{
        handleSubmit(event){
            Meteor.loginWithPassword(this.username,this.password);
        }
    },
}

</script>