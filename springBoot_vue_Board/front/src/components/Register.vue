<template>
  <div class="col-md-12">
    <div class="card card-container">
      <img
        id="profile-img"
        src="//ssl.gstatic.com/accounts/ui/avatar_2x.png"
        class="profile-img-card"
      />
      <form name="form" @submit.prevent="handleRegister">
        <div v-if="!successful">
          <div class="form-group">
            <label>Username</label>
            <input
              type="text"
              class="form-control"
              name="username"
              v-model="user.username"
              v-validate="'required|min:3|max:20'"
            />
            <div
              class="alert-danger"
              v-if="submitted && errors.has('username')"
            >{{errors.first('username')}}</div>
          </div>
          <div class="form-group">
            <label>Email</label>
            <input
              type="email"
              class="form-control"
              name="email"
              v-model="user.email"
              v-validate="'required|email|max:50'"
            />
            <div
              class="alert-danger"
              v-if="submitted && errors.has('email')"
            >{{errors.first('email')}}</div>
          </div>
          <div class="form-group">
            <label>Password</label>
            <input
              type="password"
              class="form-control"
              name="password"
              v-model="user.password"
              v-validate="'required|min:6|max:40'"
            />
            <div
              class="alert-danger"
              v-if="submitted && errors.has('password')"
            >{{errors.first('password')}}</div>
          </div>
          <div class="form-group">
            <button class="btn btn-primary btn-block">Sign Up</button>
          </div>
        </div>
      </form>

      <div
        class="alert"
        :class="successful ? 'alert-success' : 'alert-danger'"
        v-if="message"
      >{{message}}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Register',
  computed: {
    loggedIn () {
      return this.$store.state.auth.status.loggedIn
    }
  },
  data () {
    return {
      user: {username: '', email: '', password: ''},
      submitted: false,
      successful: false,
      message: ''
    }
  },

  mounted () {
    if (this.loggedIn) {
      this.$router.push('/')
    }
  },

  methods: {
    handleRegister () {
      this.message = ''
      this.submitted = true
      this.$validator.validate()
        .then(valid => {
          if (valid) {
            this.$store.dispatch('auth/register', this.user)
              .then(data => {
                this.message = data.message
                this.successful = true
              },
              error => {
                this.message = error.message
                this.successful = false
              }).then(this.$router.push('/'))
          }
        })
    }
  }
}
</script>

<style scoped>
label {
  display: block;
  margin-top: 10px;
}

.card-container.card {
  max-width: 350px !important;
  padding: 40px 40px;
}

.card {
  background-color: #f7f7f7;
  padding: 20px 25px 30px;
  margin: 0 auto 25px;
  margin-top: 50px;
  -moz-border-radius: 2px;
  -webkit-border-radius: 2px;
  border-radius: 2px;
  -moz-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  -webkit-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
}

.profile-img-card {
  width: 96px;
  height: 96px;
  margin: 0 auto 10px;
  display: block;
  -moz-border-radius: 50%;
  -webkit-border-radius: 50%;
  border-radius: 50%;
}
</style>
