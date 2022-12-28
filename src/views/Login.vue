<template>
  <div>
    <AlertError :message="errorResponse.message"/>
    <div style="width: 300px; margin: auto">
      <input v-model="name" type="text" class="form-control" placeholder="username" aria-label="username"
             aria-describedby="button-addon2">
    </div>
    <div class="input-group mb-3">
      <input v-model="password" type="password" class="form-control" placeholder="password" aria-label="username"
             aria-describedby="button-addon2">
      <div style="width: 300px; margin: auto">
        <br/>
        <button v-on:click="login" class="btn btn-outline-secondary" type="button" id="button-addon2">Login</button>
      </div>
    </div>
  </div>
</template>

<script>
import AlertError from "@/components/alert/AlertError";

export default {
  name: "Login",
  components: {AlertError},
  data: function () {
    return {
      name: '',
      password: '',

      loginResponse: {
        userId: 0
      },
      errorResponse: {
        message: '',
        errorCode: 0
      }
    }
  },

  methods: {
    login: function () {
      this.errorResponse.message = ''
      if (this.name.length == 0 || this.password.length == 0) {
        this.displayRequiredFieldsNotFilledAlert();
      } else {
        this.loginRequest();
      }
    },

    loginRequest: function () {
      this.$http.get("/login", {
        params: {
          name: this.name,
          password: this.password
        }
      }).then(response => {
        this.loginResponse = response.data
        this.navigateHome();
      }).catch(error => {
        console.log(error)
      })
    },

    navigateHome: function () {
      sessionStorage.setItem('userId', this.loginResponse.userId);
      this.$router.push({
        name: 'home', query: {
          userId: this.userId
        }
      })
    },

    displayRequiredFieldsNotFilledAlert: function () {
      this.errorResponse.message = 'Fill all inputs'
    },
  }
}
</script>

