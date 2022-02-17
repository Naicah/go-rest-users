<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <div v-for="(user, i) in users" :key="i + 'user'">
      <p>{{ user.id }}: {{ user.name }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      APIUrl: "https://gorest.co.in/public/v2/users/",
      bearerToken:
        "Bearer 89907c468eb0aaa16db84b8c185e4b33f44e7a08c64b374829d684518cec39b2",
      users: [],
    };
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
    this.getAllUsers();
    this.updateUser("4897", {
      name: "Mimmi",
    });
    this.deleteUser("3884");
    // this.getUser("3894");
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

    async getUser(id) {
      const response = await fetch(`${this.APIUrl}${id}`, {
        headers: {
          Authorization: this.bearerToken,
        },
      });
      console.log(await response.json());
      return await response.json();
    },

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

    async updateUser(id, data) {
      const response = await fetch(`${this.APIUrl}${id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
          Authorization: this.bearerToken,
        },
        body: JSON.stringify(data),
      });
      console.log(await response.json());
      this.getAllUsers();
    },

    async deleteUser(id) {
      const response = await fetch(`${this.APIUrl}${id}`, {
        method: "DELETE",
        headers: {
          Authorization: this.bearerToken,
        },
      });
      console.log(response.status);
      this.getAllUsers();
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
