<template>
  <div class="input-wrap">
    <label v-if="label">{{ label }}</label>
    <input
      class="input-form"
      type="text"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      v-bind="$attrs"
    />
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
  .input-wrap {
    display: flex;
    flex-direction: column;
  }

  .input-form {
    font-size: 16px;
    padding: 0.25rem;
    border-radius: 6px;
    border: none;
  }
</style>
