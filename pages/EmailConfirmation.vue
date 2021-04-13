<template>
  <div class="econfirm">
    <NavBar />
    <h3 class="title">
      Please confirm your account
    </h3>
    <button class="verbutton" @click="signInVerified()">
      Email already verified
    </button>
  </div>
</template>

<script>
import NavBar from '../components/Navbar.vue'
export default {
  components: {
    NavBar
  },
  methods: {
    async signInVerified () {
      await this.$axios
        .$post('/user/signin', {
          email: sessionStorage.getItem('email'),
          password: sessionStorage.getItem('password')
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
            alert('Email is not verified')
          } else {
            alert('Invalid email or password')
          }
          return true
        })
    }
  }
}
</script>

<style>
.econfirm {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.verbutton {
  display: block;
  width: 30%;
  height: 50px;
  margin: 30px auto;
  border-radius: 10px;
  background-color: black;
  color: white;
  border-color: black;
}
.verbutton:hover {
  background-color: white;
  color: black;
  cursor: pointer;
}
.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 40px;
  color: #35495e;
  letter-spacing: 1px;
  text-decoration: gold solid overline;
  margin-bottom: 20px;
}
</style>
