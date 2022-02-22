<template>
  <div class="container">
    <h1>{{ msg }}</h1>

    <div class="users">
      <div
        class="user"
        :class="{ inactive: user.status === 'inactive' }"
        v-for="(user, i) in users"
        :key="i + 'user'"
        @click="showModal(user)"
      >
        <h3>{{ user.name }}</h3>
        <p>{{ user.email }}</p>
        <p>{{ user.gender }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Users",
  props: {
    msg: String,
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
    return {};
  },

  created() {
    // this.createUser();
    // this.createUser({
    //   name: "Mimmi Melander",
    //   email: "ander@gmail.com",
    //   gender: "Female",
    //   status: "active",
    // }).then((data) => {
    //   console.log(data);
    // });
    // this.updateUser("4897", {
    //   name: "Mimmi",
    // });
    // this.deleteUser("3884");
  },

  methods: {
    async createUser(data) {
      const response = await fetch(this.APIUrl, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: this.bearerToken,
        },
        body: JSON.stringify(data),
      });
      console.log(await response.json());
    },

    showModal(user) {
      console.log("emit showUser");
      console.log("user:", user);
      this.$emit("showUser", user);
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  justify-content: space-around;
  flex-direction: column;
  align-items: center;
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
  background: #c2efdc;
  border-radius: 1rem;
  transition: transform 0.2s;
  -webkit-box-shadow: 5px 4px 7px 2px rgba(0, 0, 0, 0.4),
    5px 4px 7px 2px rgba(0, 0, 0, 0.4);
  box-shadow: 5px 4px 7px 2px rgba(0, 0, 0, 0.4),
    5px 4px 7px 2px rgba(0, 0, 0, 0.4);

  &:hover {
    cursor: pointer;
    transform: scale(1.05);
    background: #81b8a1;
  }

  &.inactive {
    opacity: 50%;
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
