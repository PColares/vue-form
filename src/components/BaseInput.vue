<template>
  <div class="flex flex-col relative w-full">
    <input
      class="w-full rounded-lg px-2.5 pb-2.5 pt-5 text-sm text-gray-900 bg-gray-50 dark:bg-gray-700
      border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600
      dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer"
      placeholder=" "
      type="text"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      v-bind="$attrs"
    />
    <label
      class="absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-4 scale-75 top-4 z-10 origin-[0] left-2.5 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-4"
      v-if="label"
      >{{ label }}</label
    >
  </div>
</template>

<script setup>
defineProps({
  label: {
    type: [String, Boolean],
    default: false
  },
  modelValue: {
    type: String,
    default: ''
  }
})
</script>

<style scoped>
/* .input-wrap {
  display: flex;
  flex-direction: column;
  position: relative;
} */

/* Quando tiver focus no input-form, vamos alterar o label */
/* input:focus + label,
input:not(:placeholder-shown) + label {
  transform: translateY(-27px);
} */

/*
 .input-wrap input:focus {
  outline: none;
  border-color: #6236ff;
  box-shadow: 0 1px 0 0 #6236ff;
} */

/* label {
  position: absolute;
  height: 100%;
  display: flex;
  align-items: center;
} */

/*
input {
  font-size: 14px;
  font-weight: 300;
  border-radius: 6px;
  border: none;
  width: 350px;
  padding: 8px 0;
  border: none;
  background-color: transparent;
  border-bottom: 1px solid #181824;
  transition: 300ms ease;
} */
</style>
