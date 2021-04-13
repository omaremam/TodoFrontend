<template>
  <div class="adminmain">
    <NavBar />
    <h3 class="title">
      Manage users
    </h3>
    <UserTile v-for="user of users" :key="user.id" :user="user" />
  </div>
</template>

<script>
import NavBar from '../components/Navbar'
import UserTile from '../components/UserTile'
export default {
  components: {
    NavBar,
    UserTile
  },
  data () {
    return {
      users: []
    }
  },
  mounted () {
    if (sessionStorage.getItem('userType') === 'ADMIN') {
      this.$axios.get('/user/regular').then((users) => {
        //* Get all regular users
        this.users = users.data
      })
    } else {
      alert('You are not authorized to view this page')
      this.$router.push({
        path: '/'
      })
    }
  }
}
</script>

<style>
.adminmain {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
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
