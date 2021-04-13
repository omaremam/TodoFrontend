/* eslint-disable vue/no-parsing-error */
<template>
  <div class="modal-backdrop">
    <div class="modal">
      <header class="modal-header">
        <div class="title">
          Add list
        </div>
        <button type="button" class="btn-close" @click="close">
          X
        </button>
      </header>

      <section class="modal-body">
        <input v-model="listName" class="infield" placeholder="List name">
      </section>

      <footer class="modal-footer">
        <button type="button" class="itembutton" @click="addItem">
          Add list
        </button>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      listName: ''
    }
  },
  methods: {
    close () {
      this.$emit('close')
    },
    async addItem () {
      await this.$axios.post(
        '/todo',
        {
          listName: this.listName
        },
        { headers: { userid: sessionStorage.getItem('userid') } }
      )
      this.$emit('close')
    }
  }
}
</script>

<style scoped>
.btn-close {
  background-color: white;
  border-radius: 12.5px;
  height: 25px;
  width: 25px;
  color: red;
  border-color: transparent;
  position: absolute;
  top: 5px;
  right: 5px;
}
.btn-close:hover {
  cursor: pointer;
  background-color: gold;
  color: white;
}
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: #f5f5f5;
  box-shadow: 2px 2px 20px 1px;
  overflow-x: auto;
  display: flex;
  flex-direction: column;
}

.modal-header,
.modal-footer {
  padding: 35px;
  display: flex;
}

.modal-header {
  position: relative;
  border-bottom: 1px solid #eeeeee;
  color: #4aae9b;
  justify-content: space-between;
  align-content: center;
}

.modal-footer {
  border-top: 1px solid #eeeeee;
  flex-direction: column;
  justify-content: center;
}

.modal-body {
  position: relative;
  padding: 20px 10px;
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
  margin: 20px 30px;
}
.infield {
  border-radius: 10px;
  border: 0;
  box-shadow: 0 0 15px 4px rgba(0, 0, 0, 0.06);
  width: 80%;
  display: block;
  margin: 20px auto;
  height: 50px;
  padding: 5px;
}
.infield:focus {
  outline: none;
}
.itembutton {
  display: block;
  width: 90%;
  height: 40px;
  margin: 0px auto;
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

</style>
