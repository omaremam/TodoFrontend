<template>
  <div v-if="!isDeleted" class="todotile">
    <p class="texttodo name">
      {{ item.itemName }}
    </p>
    <div class="rightalign">
      <input
        type="checkbox"
        class="regular-checkbox"
        :checked="isDone"
        @change="toggleDoneOnItem"
      >
      <fa id="ictrash" :icon="['fas', 'trash']" @click="deleteItem" />
    </div>
  </div>
</template>
<script>
export default {
  props: {
    item: {
      default () {
        return {}
      },
      type: Object
    }
  },
  data () {
    return {
      isDone: false,
      isDeleted: false
    }
  },
  mounted () {
    this.item.itemStatus === 'DONE'
      ? (this.isDone = true)
      : (this.isDone = false)
  },
  methods: {
    async toggleDoneOnItem () {
      await this.$axios.put(
        '/todo/done',
        {},
        {
          headers: {
            todoid: sessionStorage.getItem('todoid'),
            itemid: this.item._id
          }
        }
      )
    },
    async deleteItem () {
      await this.$axios.delete('/todo/delete', {
        headers: {
          todoid: sessionStorage.getItem('todoid'),
          itemid: this.item._id
        }
      })
      this.isDeleted = true
    }
  }
}
</script>

<style>
#ictrash {
  height: 27px;
  width: 30px;
  margin: 0px 20px;
  position: relative;
  top: 20px;
}
#ictrash:hover {
  color: goldenrod;
  cursor: pointer;
}
.regular-checkbox {
  -webkit-appearance: none;
  background-color: #fafafa;
  border: 1px solid #cacece;
  padding: 9px;
  border-radius: 3px;
  display: inline-block;
  position: relative;
  top: 20px;
  height: 30px;
  width: 30px;
}
.regular-checkbox:hover {
  background-color: goldenrod;
  cursor: pointer;
}
.regular-checkbox:checked {
  background-color: goldenrod;
  border: 1px solid #adb8c0;
  color: #99a1a7;
}
.todotile {
  display: flex;
  width: 60%;
  height: 70px;
  background-color: #f5f5f5;
  justify-content: center;
  align-items: center;
  border-radius: 20px;
  margin: 5px 0;
  color: black;
}
.todotile:hover {
  background-color: black;
  color: white;
}
.texttodo {
  font-size: 25px;
  text-align: center;
  vertical-align: middle;
  line-height: 70px;
}
.delete {
  margin: 0 20px;
}
.delete:hover {
  cursor: pointer;
}
.rightalign {
  flex: 1;
  display: flex;
  justify-content: flex-end;
  height: 70px;
}
.name {
  flex: 1;
}
</style>
