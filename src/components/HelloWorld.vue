<template>
  <v-container>
    <v-alert v-if="isAlert" type="error">
      Wrong username or password
    </v-alert>
    <v-alert v-if="isLoggedIn" type="success">
      Login successful
    </v-alert>
    <v-text-field
            v-model="username"
            label="Username"
            clearable
          ></v-text-field>
    <v-text-field
            v-model="password"
            :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
            :rules="[rules.required, rules.min, rules.max]"
            :type="show1 ? 'text' : 'password'"
            name="input-10-1"
            label="Password"
            hint="At least 8 characters"
            counter
            @click:append="show1 = !show1"
          ></v-text-field>
          <v-btn
            elevation="2"
            color="primary"
            @click="login"
          >Log in</v-btn>
          <v-btn
            elevation="2"
            color="red lighten-2"
            @click="logout"
          >Log out</v-btn>
  </v-container>
</template>

<script>
import axios from 'axios'
import cookies from 'vue-cookies'
  export default {
    name: 'HelloWorld',

    data: () => {
      return {
        username : undefined,
        show1: false,
        password: 'Password',
        isAlert : false,
        isLoggedIn : false,
        rules: {
          required: value => !!value || 'Required.',
          min: v => v.length >= 10 || 'Min 8 characters',
          max: v => v.length <= 25 || 'Maximum 25 characters',
          emailMatch: () => (`The email and password you entered don't match`),
        },
      }
    },
    methods: {
      login() {
        axios.request({
          url: "https://reqres.in/api/login",
          method: "POST",
          headers : {
            'Content-Type' : 'application/json'
          },
          data : {
            "email": this.username,
            "password": this.password
          }
        }).then((response)=>{
          cookies.set('sessionToken', response.data.token);
          this.isAlert = false;
          this.isLoggedIn = true;
          console.log(response);
        }).catch(()=>{
          this.isAlert = true;
        })
      },
      logout(){
        cookies.remove('sessionToken');
      }
    },
  }
</script>
