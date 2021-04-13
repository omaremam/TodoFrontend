<template>
  <div class="todomain">
    <h3 class="title">
      Current Todos
    </h3>
    <TodoTile v-for="item of items" :key="item._id" :item="item" />
    <button class="itembutton" @click="openModal()">
      Add an item
    </button>
    <TodoModal v-show="modalOpen" @close="closeModal" />
  </div>
</template>

<script>
import TodoTile from '../../components/TodoTile'
import TodoModal from '../../components/TodoModal'
export default {
  components: {
    TodoTile,
    TodoModal
  },
  data () {
    return {
      items: [],
      modalOpen: false
    }
  },
  mounted () {
    this.$axios
      .get('todo/id', {
        headers: { todoid: sessionStorage.getItem('todoid') }
      })
      .then((list) => {
        this.items = list.data.items
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
.itembutton {
  display: block;
  width: 30%;
  height: 50px;
  margin: 30px auto;
  border-radius: 10px;
  background-color: black;
  color: white;
  border-color: black;
}
.itembutton:hover {
  background-color: white;
  color: black;
  cursor: pointer;
}
.todomain {
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
