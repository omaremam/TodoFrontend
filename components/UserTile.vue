<template>
  <div v-if="!isDeleted" class="usertile" @click="openModal()">
    <p class="textuser">
      {{ user.email }}
    </p>
    <p class="textuser">
      {{ user.name }}
    </p>
    <p class="textuser">
      {{ user.lastAccessDate }}
    </p>
    <fa id="ictrash" :icon="['fas', 'trash']" @click="deleteUser" />
    <EditUserModal v-show="modalOpen" :user="user" @close="closeModal" />
  </div>
</template>

<script>
import EditUserModal from '../components/EditUserModal'

export default {
  components: {
    EditUserModal
  },
  props: {
    user: { default () {}, type: Object }
  },
  data () {
    return {
      isDeleted: false,
      modalOpen: false
    }
  },
  methods: {
    async deleteUser () {
      await this.$axios.delete('/user/delete', {
        headers: { id: this.user.id, adminid: sessionStorage.getItem('userid') }
      })
      this.isDeleted = true
    },
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
#ictrash {
  height: 27px;
  width: 30px;
  flex: 1;
}
#ictrash:hover {
  color: goldenrod;
  cursor: pointer;
}
.usertile {
  display: flex;
  width: 60%;
  height: 70px;
  background-color: #f5f5f5;
  align-items: center;
  border-radius: 20px;
  margin: 5px 0;
  color: black;
}
.usertile:hover {
  background-color: black;
  color: white;
  cursor: pointer;
}
.textuser {
  font-size: 15px;
  text-align: center;
  vertical-align: middle;
  line-height: 70px;
  flex: 1;
}
</style>
