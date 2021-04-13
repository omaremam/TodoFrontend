<template>
  <div class="cont">
    <div v-show="!isDefault" class="tile" @click="goToTodo">
      <p class="texttile">
        {{ list.listName }} ->
      </p>
    </div>
    <div v-show="isDefault" class="tile" @click="defaultClicked">
      <p class="texttile">
        +
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    list: {
      default () {
        return {
          itemName: '+'
        }
      },
      type: Object
    },
    isDefault: Boolean //* Checks whether its the plus component or not
  },
  methods: {
    goToTodo () {
      sessionStorage.setItem('todoid', this.list.id)
      this.$router.push({
        path: '/todo/todopage'
      })
    },
    defaultClicked () {
      this.$emit('defaultClicked')
    } //* Emit modal open event to parent
  }
}
</script>

<style>
.tile {
  height: 200px;
  display: flex;
  justify-content: center;
  align-content: center;
  background-color: #f5f5f5;
  border-radius: 20px;
  box-shadow: 3px 3px grey;
  margin: 20px 10px;
  color: black;
}
.tile:hover {
  background-color: black;
  cursor: pointer;
  box-shadow: none;
  color: white;
}
.texttile {
  font-size: 25px;
  text-align: center;
  vertical-align: middle;
  line-height: 200px;
}
.cont {
  display: inline-block;
  width: 300px;
}
</style>
