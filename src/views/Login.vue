<template>
    <div class="background">
        <form @submit.prevent="submit">
            <div class="row">
                <div class="title">WELCOME</div>
            </div>
            <div class="row" :class="{ 'hasError': $v.form.email.$error }">
                <b-form-input type="email" v-model="form.email" placeholder="Email"></b-form-input>
                <div class="invalid-feedback"></div>
            </div>
            <div class="row" :class="{ 'hasError': $v.form.password.$error }">
                <b-form-input type="password" v-model="form.password" placeholder="Password"></b-form-input>
            </div>
            <div class="row">
                <b-button type="submit" id="login">Login</b-button>
                
            </div>
            <div class="row">
                <b-button variant="primary" id="facebook">Facebook</b-button>
                <b-button variant="danger" id="google">Google</b-button>
            </div>
        </form>
    </div>
</template>
<script>
import {required, email, minLength}  from 'vuelidate/lib/validators'
import { mapState, mapActions } from 'vuex'
//import axios from 'axios';
export default {
    
  name: 'Login',
//   mounted(){
//       axios.post('https://everest-back-end.herokuapp.com/api/login',{'email':'trinhle0120@gmail.com','password':'123456'}).then((res) =>{
//           console.log(res.data);
//       })
//   },

  data(){
      return{
          form:{
              email:"",
              password:"",
          },
          submitted: false
      }
  },

  validations:{
      form:{
          email:{required, email},
          password:{required, minLength:minLength(8)}
      }
  },

  computed: {
        ...mapState('account', ['status'])
    },

  created () {
    // reset login status
    this.logout();
  },

  methods: {
    submit() {
      console.log('submit!')
      this.$v.form.$touch()
      if (this.$v.form.$error) return
      alert('Form submitted')
    },
    ...mapActions('account', ['login', 'logout']),
    handleSubmit () {
        this.submitted = true;
        const { username, password } = this;
        if (username && password) {
            this.login({ username, password })
        }
    }
  }
}
</script>

<style lang="scss" scoped>

@import "../assets/style/common.scss";

.background{
    @include absoluteCenter("both");
    @include bgcolor(#dbdbdb);
    @include size(100%);
    position: fixed;
}

form{
    @include size(500px);
    @include bgcolor(white);
    @include absoluteCenter("both");
    border-radius: 1%;
    align-items: center;
    padding: 2% 3% 2% 3%;
    box-shadow: rgba(0, 0, 0, 0.07) 0px 1px 1px, rgba(0, 0, 0, 0.07) 0px 2px 2px, rgba(0, 0, 0, 0.07) 0px 4px 4px, rgba(0, 0, 0, 0.07) 0px 8px 8px, rgba(0, 0, 0, 0.07) 0px 16px 16px;
    display: flex;
    flex-direction: column;
}

.row{
    @include size(100%,20%);
}

.title{
    @include font("title");
    width: 100%;
}

input{
    @include size(100%,50%);
    @include font("infor");
    .hasError & {
      border-color: red;
    }
}

button{
    @include size(100%,60%);
    font-weight: 600;
}

#login,#facebook,#google{
    outline: 0;
    box-shadow: none;
    color: white;
}

#login{
    @include bgcolor($mainColor);
}

#facebook,#google{
    width: 45%;
}

#google{
    margin-left: 10%;
}

.invalid-feedback{
    display: inline;
    margin-top: -5%;
}
</style>
