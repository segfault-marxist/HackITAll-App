


<template>
    <v-app dark style="background: linear-gradient(to bottom, #081B33, #0C3857);">
      <div class="logo-container">
        <img src="../assets/logo_transparent.png" height="140px" width="250px">
      </div>
      <v-content>
        <v-container fluid fill-height>
          <v-row align="center" justify="center" style="margin-top: -70px;">
            <v-col md="4">
              <v-card>
                <v-card-title class="text-center">Sign Up</v-card-title>
                <v-card-text>
                  <v-form @submit.prevent="submitForm">
                    <v-text-field
                      v-model="id"
                      label="ID Student"
                      prepend-icon="mdi-account"
                      type="id"
                      required
                      :rules="[idRule]"
                    ></v-text-field>
                    <v-text-field
                      v-model="name"
                      label="Name"
                      prepend-icon="mdi-account"
                      type="name"
                      required
                    ></v-text-field>
                    <v-text-field
                      v-model="email"
                      label="Email"
                      prepend-icon="mdi-email"
                      type="email"
                      required
                      :rules="[emailRule]"
                    ></v-text-field>
                    <v-text-field
                      v-model="password"
                      label="Password"
                      prepend-icon="mdi-lock"
                      type="password"
                      required
                    ></v-text-field>
                    <!-- Make the button full like the card -->
                    <v-btn color="primary" block type="submit">Sign up</v-btn>
                  </v-form>
                </v-card-text>
              </v-card>
              <div>
                <p>Already have an account? <nuxt-link to="/login">Log in</nuxt-link></p>
              </div>
            </v-col>
          </v-row>
        </v-container>
      </v-content>
    </v-app>
  </template>
  
  <script>
    import Vue from 'vue'
    import Vuetify from 'vuetify'
  
    Vue.use(Vuetify)
  
    export default {
      layout: "no-layout",
      data() {
        return {
          name: '',
          id: '',
          email: '',
          password: '',
        }
      },
      methods:{

        idRule: (value) => {
          if (!/^\d{8}$/.test(value)) {
            return 'ID must be an 8 digit string';
          } else {
            return true;
          }
        },

        emailRule: (value) => {
          if (!/@stud.*upb\.ro$/i.test(value)) {
            return 'email is incorrect';
          } else {
            return true;
          }
        },

        submitForm(){
          try{
            // send data as json
            const str = this.id;
            const isEightDigits = /^\d{8}$/.test(str);
           

            const email = this.email;
            const hasUpbStudDomain = /@stud.*upb\.ro$/i.test(email);
            

            if(isEightDigits && hasUpbStudDomain){

            const response = this.$axios.$post('/api/user', JSON.stringify(
              {
                id: this.id,
                name: this.name,
                email: this.email,
                password: this.password,
                balance: 0.0
              }
            ), {
              headers: {
                'Content-Type': 'application/json'
              }
            })
            console.log(
              JSON.stringify(
                {
                  name: this.name,
                  id: this.id,
                  email: this.email,
                  password: this.password
                }
              )
            )
            // Store this in local storage
            localStorage.setItem('user', JSON.stringify({
                id: this.id,
                name: this.name,
                email: this.email,
                password: this.password,
                balance: 0.0
              }))

            // Redirect to the home page
            this.$router.push('/')
            }
          }catch(err){
            console.log(err)
          }
      }
    }
  }
  </script>

  <style>
  .logo-container {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 50px;
  }

  .v-messages__message {
  color: red;
  }
  </style>