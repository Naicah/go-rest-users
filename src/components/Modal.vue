<template>
  <div class="modal">
    <div class="box">
      <h2>User {{ user.id }}</h2>
      <div class="inputs">
        <Input
          :data="user.name"
          :label="'Name'"
          :placeholder="'Name'"
          @updateValue="name = $event"
        />
        <Input
          :data="user.email"
          :label="'Email'"
          :placeholder="'Email'"
          @updateValue="email = $event"
        />
        <Input
          :data="user.gender"
          :label="'Gender'"
          :placeholder="'Gender'"
          @updateValue="gender = $event"
        />
        <Input
          :data="user.status"
          :label="'Status'"
          :placeholder="'Status'"
          @updateValue="status = $event"
        />
      </div>
      <p v-show="this.showError">Woops! Something went wrong</p>
      <div class="buttons">
        <button
          v-if="this.name || this.email || this.gender || this.status"
          type="button"
          class="save"
          @click="getUpdateData"
        >
          Save changes
        </button>
        <button type="button" class="delete" @click="deleteUser(user.id)">
          Delete user
        </button>
        <button type="button" @click="modalClose">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
import Input from "./Input.vue";

export default {
  name: "Modal",
  components: {
    Input,
  },
  props: {
    user: {
      type: Object,
      required: true,
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
      name: "",
      email: "",
      gender: "",
      status: "",
      showError: false,
    };
  },
  methods: {
    async getUser(id) {
      const response = await fetch(`${this.APIUrl}${id}`, {
        headers: {
          Authorization: this.bearerToken,
        },
      });
      this.user = await response.json();
    },

    getUpdateData() {
      let data = {};
      if (this.name) data.name = this.name;
      if (this.email) data.email = this.email;
      if (this.gender) data.gender = this.gender;
      if (this.status) data.status = this.status;
      this.updateUser(this.user.id, data);
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
      console.log(response);
      if (response.status == 200) {
        console.log("done");
        this.showError = false;
        this.modalClose();
      } else {
        this.showError = true;
      }
    },

    async deleteUser(id) {
      const response = await fetch(`${this.APIUrl}${id}`, {
        method: "DELETE",
        headers: {
          Authorization: this.bearerToken,
        },
      });
      console.log(response.status);
      if (response.status == 204) {
        console.log("done");
        this.showError = false;
        this.modalClose();
      } else {
        this.showError = true;
      }
    },

    modalClose() {
      this.$emit("modalClose");
      this.name = "";
      this.email = "";
      this.gender = "";
      this.status = "";
      this.showError = false;
    },
  },
  watch: {
    userID() {
      this.getUser(this.userID);
    },
  },
};
</script>

<style lang="scss" scoped>
.modal {
  display: block;
  width: 100vw;
  height: 100vh;
  background-color: rgba(94, 94, 94, 0.4);
  backdrop-filter: blur(2px);
  overflow: hidden;

  .box {
    margin: 45vh auto;
    width: 90vw;
    height: auto;
    padding: 1.5rem 2rem;
    background-color: white;
    transform: translateY(-50%);
    box-shadow: 5px 10px 18px #888888;

    h2 {
      text-align: center;
    }
    p {
      font-size: 1.2rem;
      padding-top: 1rem;
    }

    .message {
      padding: 1rem 0;
    }

    .inputs {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-around;
    }

    .buttons {
      display: flex;
      justify-content: flex-end;
      flex-direction: column;

      button {
        margin: 1rem;
        margin-bottom: 0;
        padding: 6px 20px;
        border-radius: calc(1.5em + 0.75rem + 2px);
        background-color: #9c9d99;
        border: none;
        transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out,
          border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out,
          -webkit-box-shadow 0.15s ease-in-out;
        color: white;
        font-weight: 500;
        letter-spacing: 0.3px;
        font-size: 1rem;
        line-height: 1.5;
        &:hover {
          background-color: #8a8a8a;
          cursor: pointer;
        }
        &.save {
          background-color: #4dcb96;
          &:hover {
            background-color: #61f7b8;
          }
        }
        &.delete {
          background-color: #cb4d4d;
          &:hover {
            background-color: #f76161;
          }
        }
      }
    }
  }
}

/* Small devices (portrait tablets and large phones, 600px and up) */
@media only screen and (min-width: 600px) {
  .modal .box .buttons {
    flex-direction: row;
  }
}

/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (min-width: 992px) {
  .modal .box {
    width: 80vw;
    padding: 2rem 4rem;
  }
}
</style>
