<template>
  <div class="main">
    <h3 class="title">
      {{ name }} Lists
    </h3>
    <div>
      <ListTile
        v-for="list of lists"
        :key="list.id"
        :list="list"
        :is-default="false"
      />
      <ListTile :is-default="true" @defaultClicked="openModal()" />
      <AddListModal v-show="modalOpen" @close="closeModal" />
    </div>
  </div>
</template>

<script>
import ListTile from '../../components/ListTile'
import AddListModal from '../../components/AddListModal'
export default {
  components: {
    ListTile,
    AddListModal
  },
  data () {
    return {
      name: '',
      lists: [],
      modalOpen: false
    }
  },
  mounted () {
    this.name = sessionStorage.getItem('name')
    this.$axios
      .get('/todo/user', {
        headers: { userid: sessionStorage.getItem('userid') }
      })
      .then((lists) => {
        this.lists = lists.data
      })
  },
  methods: {
    openModal () {
      this.modalOpen = true
    },
    closeModal () {
      this.modalOpen = false
      this.$router.go()
    }
  }
}
</script>

<style>
.main {
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
