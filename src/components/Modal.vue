<template>
  <div class="modal">
    <div class="box card">
      <p>{{ user.id }}</p>
      <p>{{ user.name }}</p>
      <p>{{ user.email }}</p>
      <p>{{ user.gender }}</p>
      <p>{{ user.status }}</p>
      <div class="buttons">
        <button
          type="button"
          class="btn btn-rounded btn-secondary"
          @click="modalCancel"
        >
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "modal",

  props: {
    userID: {
      type: Number,
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
      user: {},
    };
  },
  methods: {
    modalCancel() {
      this.$emit("modalCancel");
      this.user = {};
    },
    async getUser(id) {
      const response = await fetch(`${this.APIUrl}${id}`, {
        headers: {
          Authorization: this.bearerToken,
        },
      });
      this.user = await response.json();
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
    width: 60%;
    height: auto;
    padding: 2rem 4rem;
    background-color: white;
    transform: translateY(-50%);
    box-shadow: 5px 10px 18px #888888;

    p {
      font-size: 1.2rem;
      padding-top: 1rem;
    }

    .message {
      padding: 1rem 0;
    }

    .buttons {
      display: flex;
      justify-content: flex-end;

      button {
        margin: 1rem;
        margin-bottom: 0;
      }
    }
  }

  @media only screen and (max-width: 600px) {
    .box {
      margin: 45vh auto;
      width: 95%;
      padding: 1.5rem 2rem;

      .buttons button {
        margin: 1.5rem 0.2rem 0.5rem 0.2rem;
      }
    }
  }
}
</style>
