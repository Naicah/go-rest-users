<template>
  <div>
    <span class="label">{{ label }}</span>
    <input v-model="inputData" :placeholder="placeholder" />
  </div>
</template>

<script>
export default {
  name: "Input",

  props: {
    iteration: {
      type: Number,
    },
    data: {
      type: String,
    },
    label: {
      type: String,
    },
    placeholder: {
      type: String,
    },
  },
  data() {
    return {
      inputData: "",
    };
  },
  methods: {},
  watch: {
    iteration() {
      this.inputData = this.data;
    },
    inputData() {
      if (this.inputData !== this.data) {
        this.$emit("updateValue", this.inputData);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
div {
  width: 100%;
  .label {
    font-size: 0.8rem;
    color: grey;
    margin-top: 1rem;
  }
  input {
    width: 100%;
    padding: 0.8rem 1.2rem;
    margin: 0.3rem 0 1rem 0;
    border-radius: 0.3rem;
    border: 2px solid #e8fbf3;
    &:focus-visible {
      outline-color: #97f1cc;
    }
  }
}
/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (min-width: 992px) {
  div {
    width: 45%;
  }
}
</style>
