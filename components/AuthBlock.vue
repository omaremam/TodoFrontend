/* eslint-disable no-console */
<template>
  <div class="authblock">
    <input v-model="email" class="infield" placeholder="Email">
    <input
      v-model="password"
      class="infield"
      placeholder="Password"
      type="password"
    >
    <input
      v-show="isRegister"
      v-model="name"
      class="infield"
      placeholder="Name"
    >
    <button v-show="!isRegister" class="authbutton" @click="signIn()">
      Sign in
    </button>
    <button v-show="isRegister" class="authbutton" @click="register()">
      Register
    </button>

    <a v-show="!isRegister" @click="toggle">
      Don't have an account? Register now
    </a>
    <a v-show="isRegister" @click="toggle">
      Already have an account? Sign in
    </a>
  </div>
</template>

<script>
export default {
  data () {
    return {
      email: '',
      password: '',
      name: '',
      isRegister: false
    }
  },
  methods: {
    toggle () {
      this.isRegister = !this.isRegister
    },
    async signIn () {
      await this.$axios
        .$post('/user/signin', {
          email: this.email,
          password: this.password
        })
        .then((user) => {
          sessionStorage.setItem('email', user.email)
          sessionStorage.setItem('name', user.name)
          sessionStorage.setItem('userType', user.userType)
          sessionStorage.setItem('userid', user.id)
          if (user.userType === 'REGULAR') {
            this.$router.push({
              path: '/todo/mainpage'
            })
          } else {
            this.$router.push({
              path: '/admin'
            })
          }
        })
        .catch((err) => {
          if (err.response.status === 409) {
            sessionStorage.setItem('email', this.email)
            sessionStorage.setItem('password', this.password)
            this.$router.push({
              path: '/emailconfirmation'
            })
          } else {
            alert('Invalid email or password')
          }
          return true
        })
    },
    async register () {
      await this.$axios
        .$post('/user/register', {
          email: this.email,
          password: this.password,
          name: this.name,
          userType: 'REGULAR'
        })
        .then((_) => {
          sessionStorage.setItem('email', this.email)
          sessionStorage.setItem('name', this.name)
          sessionStorage.setItem('password', this.password)
          this.$router.push({
            path: '/emailconfirmation'
          })
        })
        .catch((err) => {
          alert(err.response.data.errors[0])
        })
    }
  }
}
</script>

<style>
a:hover {
  cursor: pointer;
  color: blue;
}
.authblock {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  width: 40vw;
  height: 60vh;
  background-color: #f5f5f5;
  margin: 30px;
  border-radius: 20px;
}

.authbutton {
  display: block;
  width: 80%;
  height: 50px;
  margin: 30px auto;
  border-radius: 10px;
  background-color: black;
  color: white;
  border-color: black;
}

.authbutton:hover {
  background-color: white;
  color: black;
  cursor: pointer;
}

.infield {
  padding: 10px;
  border-radius: 10px;
  border: 0;
  box-shadow: 0 0 15px 4px rgba(0, 0, 0, 0.06);
  width: 80%;
  display: block;
  margin: 20px auto;
  height: 50px;
}
.infield:focus {
  outline: none;
}
</style>
