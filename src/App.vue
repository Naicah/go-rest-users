<template>
  <div id="app">
    <Users
      msg="Go REST Users"
      :users="users"
      :APIUrl="APIUrl"
      :bearerToken="bearerToken"
      @showUser="showUser"
    />
    <transition name="fade" mode="out-in">
      <Modal
        class="modal"
        v-show="showModal"
        @modalClose="hideModal"
        :user="chosenUser"
        :APIUrl="APIUrl"
        :bearerToken="bearerToken"
      />
    </transition>
  </div>
</template>

<script>
import Users from "./components/Users.vue";
import Modal from "./components/Modal.vue";

export default {
  name: "App",
  components: {
    Users,
    Modal,
  },
  data() {
    return {
      APIUrl: "https://gorest.co.in/public/v2/users/",
      bearerToken:
        "Bearer 89907c468eb0aaa16db84b8c185e4b33f44e7a08c64b374829d684518cec39b2",
      showModal: false,
      users: [],
      chosenUser: {},
    };
  },
  created() {
    this.getAllUsers();
  },
  methods: {
    async getAllUsers() {
      const response = await fetch(this.APIUrl, {
        headers: {
          Authorization: this.bearerToken,
        },
      });
      this.users = await response.json();
    },
    showUser(e) {
      console.log("showUser called");
      console.log("e:", e);
      this.showModal = true;
      this.chosenUser = e;
    },
    hideModal() {
      this.getAllUsers();
      this.showModal = false;
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.modal {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1050;
  display: none;
  width: 100%;
  height: 100%;
  overflow: hidden;
  outline: 0;
}
</style>
