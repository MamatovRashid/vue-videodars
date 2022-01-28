<template>
  <div class="bg-gray-100 min-h-screen">
    <div class="max-w-xl mx-auto pt-20">
      <h1 class="text-8xl text-center text-blue-600 mb-4">todos</h1>
      <div class="bg-white shadow-xl">
        <div class="flex items-center">
          <button class="transform text-2xl -rotate-90 px-3 text-gray-300">&#10094;</button>
          <input v-model="inputTodo" @keydown.enter="addTodos" type="text" class="text-2xl p-3 outline-none flex-1" placeholder="What needs to be done" />
        </div>
        <ul class="border-t">
          <todo v-for="todo in todos" :key="todo.id" :todo="todo" @done-change="changeDone(todo.id)" />
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import todo from "./components/todo.vue";
export default {
  components: { todo },
  name: "Todo",
  data: () => ({
    todos: [],
    inputTodo: "",
  }),
  methods: {
    addTodos() {
      if (this.inputTodo) {
        this.todos.push({
          id: this.todos.length + 1,
          text: this.inputTodo,
          done: false,
        });
        this.inputTodo = "";
      }
    },
    changeDone(todoId) {
      this.todos = this.todos.map((todo) => {
        if (todo.id === todoId) {
          return { ...todo, done: !todo.done };
        } else {
          return todo;
        }
      });
    },
  },
};
</script>

<style lang="scss" scoped></style>
