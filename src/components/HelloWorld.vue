<template>
    <v-container>
        <v-sheet
                class="mx-auto pa-12 ma-12"
                elevation="0"
                 color="grey lighten-5"
            >

            <v-layout row wrap align-center>
                <v-flex>
                    <v-card width="400" class="mx-auto" floating>
                        <v-card-title>
                            <h1 class="display-1 mx-auto font-weight-light">Login</h1>
                            
                        </v-card-title>

                        <v-card-text>
                            
                            <v-form  @submit.prevent="loginPost()" >
                                <v-text-field
                                    name="email"
                                    label="Email"
                                    id="email"
                                    v-model="loginDetails.email"
                                    :rules="emailRules"
                                ></v-text-field>
                                <v-text-field
                                    name="password"
                                    label="Password"
                                    id="password"
                                    v-model="loginDetails.password"
                                    :type="showPassword ? 'text' : 'password'"
                                    :rules="passwordRules"

                                ></v-text-field>

                                <v-divider></v-divider>
                                <p v-if="loginDetailsError.error" class="text-danger mt-1 red--text lighten-1 text-center">{{ loginDetailsError.loginError }}</p>
                                <p v-if="loginDetailsValid.valid" class="text-success mt-1 green--text lighten-1 text-center">{{ loginDetailsValid.validMessage }}</p>

                                <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn color="success" type="submit" text @click="loading=true">
                                        Sign In
                                    </v-btn>
                                    
                                </v-card-actions>
                                <v-chip outlined small>api(post): {{api}}</v-chip>

                            </v-form>
                        </v-card-text>
                    </v-card>
                </v-flex>
            </v-layout>

            <v-dialog v-model="loading" fullscreen >
                <v-container fluid fill-height style="background-color: rgba(255, 255, 255, 0.5);">
                <v-layout justify-center align-center>
                    <v-progress-circular
                    indeterminate
                    color="primary">
                    </v-progress-circular>
                </v-layout>
                </v-container>
            </v-dialog>


        

        </v-sheet>
    </v-container>
</template>

<script>
  export default {

    name: 'HelloWorld',

    data: () => ({
        showPassword:false,
        loginDetails : {  email: '', password: '' },
        loginDetailsError : { error:false, loginError: '' },
        loginDetailsValid : { valid:false, validMessage: '' },
        emailRules: [
            v => !!v || 'E-mail is required',
            v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
        ],
        passwordRules: [
            v => !!v || 'Password is required',
        ],
        loading:false,
        api: 'http://localhost:8001/api/v1/login'
    }),

    mounted() {
    },

    methods: {

      loginPost(){


              var _this = this;
              this.$http.post(this.api,  this.loginDetails)
              .then(function () {
                  console.log('success!');
                //   console.log(response.data.access_token);
                  // window.location.href = '/home/home?token='+response.data.access_token;
                //   let token = response.data.access_token;
                //   localStorage.setItem('token', token);
                //   _this.$store.dispatch('checkIsLoggedIn')
                  // window.location.href = '/';
                  
                  _this.loading=false;
                  _this.loginDetailsError = { error:false, loginError: "" };
                  _this.loginDetailsValid = { valid:true, validMessage: 'Successful! Please wait..' }
                  
                //   _this.$router.push('/admin/dashboard');
                  // _this.$router.go(-1);
              })
              .catch(function (error) {

                //   console.log('failure !');
                  // console.log(error.response);
                  // console.log(error.response.data.error);
                  // _this.loginDetailsError = { error:true, loginError: error.response.data.error };
                  _this.loginDetailsError = { error:true, loginError: error.response };
                  _this.loading=false;

              })
      },
    },
  }
</script>
