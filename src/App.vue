<template>
  <div id="app">
    <h1>Go REST users</h1>

    <Users
      :users="users"
      :APIUrl="APIUrl"
      :bearerToken="bearerToken"
      @showModal="showModal"
    />
    <transition name="fade" mode="out-in">
      <Modal
        class="modal"
        v-show="displayModal"
        @modalClose="hideModal"
        :iteration="modalIteration"
        :user="chosenUser"
        :type="modalType"
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
      displayModal: false,
      users: [],
      chosenUser: {},
      modalType: "",
      modalIteration: 0,
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

    showModal(type, user) {
      this.modalIteration++;

      this.displayModal = true;
      this.modalType = type;
      this.chosenUser = user;
    },

    hideModal() {
      this.getAllUsers();
      this.displayModal = false;
    },
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  h1 {
    text-align: center;
  }
}

button {
  margin: 1rem;
  margin-bottom: 0;
  padding: 6px 20px;
  border-radius: calc(1.5em + 0.75rem + 2px);
  border: none;
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out,
    border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out,
    -webkit-box-shadow 0.15s ease-in-out;
  color: white;
  font-weight: 500;
  letter-spacing: 0.3px;
  font-size: 1rem;
  line-height: 1.5;
  background-color: #9c9d99;
  &:hover {
    filter: brightness(1.2);
    cursor: pointer;
  }
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
