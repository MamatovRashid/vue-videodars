<template>
  <div class="bg-gray-100 min-h-screen">
    <div class="max-w-xl mx-auto pt-20">
      <h1 class="text-8xl text-center text-blue-600 mb-4">todos</h1>
      <div class="bg-white shadow-xl">
        <div class="flex items-center">
          <button @click="allTodosDone" class="transform text-2xl -rotate-90 px-3 text-gray-300" :class="{ 'text-gray-900': allTodosCompleted }">&#10094;</button>
          <input v-model="inputTodo" @keydown.enter="addTodos" type="text" class="text-2xl p-3 outline-none flex-1" placeholder="What needs to be done" />
        </div>
        <ul class="border-t">
          <todo v-for="(todo, index) in filteredTodos" :key="todo.id" :todo="todo" @done-change="changeDone(todo.id)" @delete="deleteTodo(index)" @edit="editText(todo.id, text)" />
        </ul>
        <div class="flex items-center p-2 text-gray-500">
          <div class="flex-1">
            <p>{{ activeCountText }}</p>
          </div>
          <div>
            <input type="radio" class="hidden" v-model="filter" id="ALL" value="ALL" />
            <label class="py-0.5 px-1.5 mx-1 border rounded border-transparent cursor-pointer" :class="{ 'border-gray-200': filter === 'ALL' }" for="ALL">All</label>
            <input type="radio" class="hidden" v-model="filter" id="ACTIVE" value="ACTIVE" />
            <label class="py-0.5 px-1.5 mx-1 border rounded border-transparent cursor-pointer" :class="{ 'border-gray-200': filter === 'ACTIVE' }" for="ACTIVE">Active</label>
            <input type="radio" class="hidden" v-model="filter" id="COMPLETED" value="COMPLETED" />
            <label class="py-0.5 px-1.5 mx-1 border rounded border-transparent cursor-pointer" :class="{ 'border-gray-200': filter === 'COMPLETED' }" for="COMPLETED">Completed</label>
          </div>
          <div class="flex-1 flex justify-end">
            <button @click="clearDoneTodos" v-show="doneTodos">Clear completed</button>
          </div>
        </div>
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
    left: "",
    filter: "ALL",
  }),
  computed: {
    doneTodos() {
      if (this.todos.filter((todo) => todo.done).length) {
        return true;
      }
      return false;
    },
    allTodosCompleted() {
      if (this.todos.filter((todo) => !todo.done).length || !this.todos.length) {
        return false;
      }
      return true;
    },
    activeCountText() {
      const count = this.todos.filter((todo) => !todo.done).length;
      if (count === 1) {
        return "1 item left";
      }
      return `${count} items left`;
    },
    filteredTodos() {
      if (this.filter === "ALL") {
        return this.todos;
      }
      return this.todos.filter((todo) => (this.filter === "ACTIVE" ? todo.done : !todo.done));
    },
  },
  methods: {
    addTodos() {
      if (this.inputTodo) {
        this.todos.push({
          id: this.todos.length + 2,
          text: this.inputTodo,
          done: false,
        });
        this.inputTodo = "";
      }
    },
    allTodosDone() {
      const doneTodo = this.todos.filter((todo) => todo.done).length === this.todos.length;
      this.todos = this.todos.map((todo) => {
        return { ...todo, done: doneTodo ? false : true };
      });
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
    clearDoneTodos() {
      this.todos = this.todos.filter(todo => !todo.done);
    },
    editText(todoId, e) {
      console.log(todoId, e);
      // this.todos = this.todos.map((todo) => {
      //   if (todo.id === todoId) {
      //     return { ...todo, text: event };
      //   } else {
      //     return todo;
      //   }
      // });
    },
    // filterActive(){
    //   this.todos = this.todos.filter(todo =
    // },
    deleteTodo(index) {
      this.todos.splice(index, 1);
    },
  },
};
</script>

<style lang="scss" scoped></style>
