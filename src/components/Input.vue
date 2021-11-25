<template>
  <div class="mt-1">
    <input 
    class="block w-full shadow-sm sm:text-sm focus:ring-pink-500 focus:border-pink-500 border-gray-300 rounded-md" 
    :id="id"
    :type="type"
    :name="name"
    :value="modelValue"
    :required="required"
    @input="handleInput"
    />
  </div>
</template>

<script>
export default {
  props: ["type", "id", "name", "required", "modelValue"],
  methods: {
    handleInput(e) {
      this.$emit("set-input", e.target.value)
    }
  }
}
</script>