<script>
import { reactive } from "vue";

export default {
  emits: ["create-todo"],
  data() {
    return {
      todoState: reactive({
        todo: "",
        invalid: null,
        errMsg: "",
      }),
    };
  },
  methods: {
    createTodo() {
      this.todoState.invalid = false;
      if (this.todoState.todo !== "") {
        this.$emit("create-todo", this.todoState.todo);
        this.todoState.todo = "";
        return;
      }
      this.todoState.invalid = true;
      this.todoState.errMsg = "Todo value cannot be empty!";
    },
  },
};
</script>

<template>
  <div class="input-wrap" :class="{ 'input-err': todoState.invalid }">
    <input @keyup.enter="createTodo()" type="text" v-model="todoState.todo" />
    <button @click="createTodo()">Create</button>
  </div>
  <p v-show="todoState.invalid" class="err-msg">{{ todoState.errMsg }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-err {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    font-size: 20px;
    border: none;

    &:focus {
      outline: none;
    }
  }

  button {
    padding: 8px 16px;
    font-size: 1.2rem;
    border: none;
    &:hover {
      color: #41b080;
    }
  }
}
.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>
