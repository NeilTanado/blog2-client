<template>
  <div class="valign-wrapper center-align" style="margin: 5% 0 0 0;">
    <div class="row" style="width:50%;">
      <h4> <b>Sign In</b> </h4>
      <br>
      <hr>
      <br>
        <div class="input-field col s12">
          <input type="email" class="validate" v-model="email">
          <label for="email">Email</label>
        </div>
        <div class="input-field col s12" style="margin: 0 0 10% 0;">
          <input type="password" class="validate" v-model="password" >
          <label for="password">Password</label>
        </div>
        <div class="container" style="width: 100%;">
          <div class="col s12" >
            <div class="col s6">
              <a class="waves-effect waves-light btn-block btn btn-large" style="width: 100%;" @click="login()" ><i class="material-icons left">account_circle</i>Login</a><br><br>
            </div>
            <div class="col s6" >
              <a class="btn waves-effect waves-light btn-block btn-large" style="width: 100%;"><i class="material-icons left">cancel</i>Cancel</a>
            </div>
          </div>
        </div>
        <div class="col s12">
          <br>
          <p>need register? <a href="#"><router-link :to="{path:'/register'}">click me!</router-link></a></p>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import vuex from 'vuex'

export default {
  name: 'signin',
  data(){
    return{
      password:'',
      email:''
    }
  },
  methods:{
    login:function(){
      axios.post('https://blogs2.neil.guru/users/signin',{
        email : this.email,
        password : this.password
      })
      .then((value) => {
        console.log(value.data.userId);
        this.$store.commit('changeStatusLogin',true)
        localStorage.setItem('token',value.data.token)
        localStorage.setItem('userId',value.data.userId)
        this.$router.push('/')
      })
      .catch((err) => {
        console.log(err);
      })
    }
  }
}
</script>

<style>
</style>
