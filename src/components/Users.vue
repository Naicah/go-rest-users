<template>
  <div class="container">
    <div class="actions">
      <div class="search">
        <p>Find user</p>
        <Input :placeholder="'User ID'" @updateValue="userID = $event" />
      </div>
      <button type="button" @click="getUser(userID)">Search</button>
      <p v-show="findError">{{ findError }}</p>
    </div>

    <div class="users">
      <div
        class="user"
        :class="{ inactive: user.status === 'inactive' }"
        v-for="(user, i) in users"
        :key="i + 'user'"
        @click="showModal('update', user)"
      >
        <h3>{{ user.name }}</h3>
        <p>{{ user.email }}</p>
        <p>{{ user.gender }}</p>
      </div>
    </div>
    <div class="actions">
      <button type="button" @click="showModal('create', {})">Add user</button>
    </div>
  </div>
</template>

<script>
import Input from "./Input.vue";

export default {
  name: "Users",
  components: {
    Input,
  },
  props: {
    users: {
      type: Array,
    },
    APIUrl: {
      type: String,
      required: true,
    },
    bearerToken: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      userID: 0,
      findError: "",
    };
  },

  methods: {
    async getUser(id) {
      const response = await fetch(`${this.APIUrl}${id}`, {
        headers: {
          Authorization: this.bearerToken,
        },
      });
      if (response.status == 200) {
        this.showModal("update", await response.json());
        this.findError = "";
      } else if (response.status == 404) {
        this.findError = "Invalid user ID";
      }
    },
    showModal(type, user) {
      this.$emit("showModal", type, user);
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  align-items: center;
  flex-direction: column;
  .actions {
    width: 80%;
    padding: 2vw;
    display: flex;
    flex-direction: column;
    align-items: center;
    .search {
      width: 100%;
      display: flex;
      flex-direction: row;
      div input {
        margin-bottom: 0;
      }
    }
    button {
      width: 90%;
    }
    p {
      width: 40%;
      margin-left: 5vw;
    }
  }
}

.users {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  width: 80vw;
}
.user {
  display: flex;
  justify-content: space-around;
  flex-direction: column;
  align-items: center;

  width: 100%;
  margin: 5vw;
  padding: 1rem;

  font-size: 0.9rem;
  word-break: break-word;
  background: #2c8660;
  color: white;
  border-radius: 1rem;
  transition: transform 0.2s;
  -webkit-box-shadow: 5px 4px 7px 2px #b9f1d9, 5px 4px 7px 2px #b9f1d9;
  box-shadow: 5px 4px 7px 2px #b9f1d9, 5px 4px 7px 2px #b9f1d9;

  &:hover {
    cursor: pointer;
    transform: scale(1.05);
    filter: brightness(1.2);
  }

  &.inactive {
    opacity: 65%;
  }
  h3 {
    font-size: 1.1rem;
    margin-bottom: 0;
  }

  p {
    margin-bottom: 0;
  }
}

/* Small devices (portrait tablets and large phones, 600px and up) */
@media only screen and (min-width: 600px) {
  .container {
    .actions {
      flex-direction: row;
      justify-content: space-around;
      .search {
        width: 40vw;
        display: flex;
        flex-direction: row;
        div input {
          margin-bottom: 0;
        }
        * {
          width: 20vw;
        }
      }
      button {
        width: 20vw;
        margin-top: 0;
      }
    }
  }
  .user {
    width: 45%;
    margin: 2%;
  }
}

/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (min-width: 992px) {
  .users {
    width: 90vw;
  }
  .user {
    width: 20%;
  }
}
</style>
