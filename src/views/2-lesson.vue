<template>
  <div class="bg-gray-100 min-h-screen">
    <div class="max-w-xl mx-auto pt-20">
      <h1 class="text-8xl text-center text-blue-600 mb-4">todos</h1>
      <div class="bg-white ">
        <div class="flex items-center">
          <i></i>
          <input type="text" class="text-2xl p-3 outline-none flex-1" placeholder="What needs to be done" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Todo",
};
</script>

<style lang="scss" scoped></style>
