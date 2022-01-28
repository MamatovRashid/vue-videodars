<template>
  <li class="text-xl flex items-center border-b todo">
    <input type="checkbox" class="hidden" :id="todo.id" :checked="todo.done" @change="onCheckboxChange"/>
    <label :for="todo.id" class="w-9 h-9 border cursor-pointer flex items-center justify-center rounded-full mx-2"><span v-show="todo.done" class="text-green-500">&#10003;</span></label>
    <div class="py-4 flex-1 border-r border-l border-transparent" :class="{ 'border-gray-200': edit }">
      <input class="outline-none w-full" v-show="edit" @blur="edit = false" type="text" v-model="todo.text" />
      <p v-show="!edit" @dblclick="edit = true" class="w-full h-full " :class="{'text-gray-300 line-through': todo.done}">{{ todo.text }}</p>
    </div>
    <button class="mr-4 cancel" v-show="!edit">&#10005;</button>
  </li>
</template>

<script>
export default {
  name: "todo",
  data: () => ({
    edit: false,
  }),
  props: ["todo"],
  methods: {
      onCheckboxChange(){
          this.$emit('done-change')
      }
  }
};
</script>

<style lang="scss" scoped>
.cancel {
  display: none;
}
.todo:hover .cancel {
  display: block;
}
</style>
