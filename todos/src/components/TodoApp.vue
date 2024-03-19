<template>
  <div class="main">
    <h1>Todo app</h1>
    <div class="m_b">
      <form @submit.prevent="handleSubmit">
        <input type="text" v-model="todoText" placeholder="Enter todo text" />
        <button type="submit">Add Todo</button>
      </form>
    </div>
    <TodoTable :data="todos" @delete="handleDelete" />
  </div>
</template>

<script>
import TodoTable from "./Table.vue";
import { nanoid } from "nanoid";

export default {
  name: "TodoApp",
  components: {
    TodoTable,
  },
  data() {
    return {
      todoText: "",
      todos: [],
    };
  },
  mounted() {
    const storedTodos = localStorage.getItem("todos");
    if (storedTodos) {
      this.todos = JSON.parse(storedTodos);
    }
  },
  methods: {
    handleSubmit() {
      if (this.todoText.trim() !== "") {
        const uid = nanoid();
        this.todos.push({
          uid: uid,
          checked: false,
          text: this.todoText,
        });
        this.saveTodos();
      }
      this.todoText = "";
    },
    handleDelete(index) {
      this.todos.splice(index, 1);
      this.saveTodos();
    },
    toggleCheckbox(todo) {
      todo.completed = !todo.completed;
      this.saveTodos();
    },
    toggleEdit(todo) {
      todo.editing = !todo.editing;
      if (todo.editing) {
        todo.originalText = todo.text;
        todo.editedText = todo.text;
      }
    },
    saveEdit(todo) {
      if (todo.editedText.trim() !== "") {
        todo.editing = false;
        todo.text = todo.editedText;
        this.saveTodos();
      } else {
        todo.editing = false;
        todo.editedText = todo.originalText;
      }
    },
    saveTodos() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
};
</script>

<style scoped>

.main {
  background: #988d8d5c;
  margin-top: 30px;
  width: 60%;
  min-width: 500px;
  margin: 30px auto;
  padding: 10px;
  border: transparent;
  border-radius: 13px;
}

input {
  /* width: 470px; */
  width: 60%;
  font-size: 17px;
  padding: 9px;
  border: transparent;
  border-top-left-radius: 20px;
  border-bottom-left-radius: 20px;
}

.m_b {
  margin-bottom: 20px;
}

input {
  height: 30px;
  padding-left: 15px;
  padding-right: 5px;
}

button {
  padding: 11px;
  color: #ffffff;
  border: transparent;
  background: rgb(192, 153, 82);
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
  border: none;
  height: 48px;
  width: 100px;
}

</style>
