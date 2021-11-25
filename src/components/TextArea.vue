<template>
  <div class="mt-1">
    <textarea 
    class="block w-full shadow-sm sm:text-sm focus:ring-pink-500 focus:border-pink-500 border border-gray-300 rounded-md"
    :id="id"
    :name="name"
    :rows="rows"
    :required="required"
    :value="modelValue"
    @input="handleInput"
    >
    </textarea>
  </div>

</template>

<script>
export default {
  props: ["id", "name", "rows", "required", "modelValue"],
  methods: {
    handleInput(e) {
      this.$emit("set-textarea", e.target.value)
    }
  }
}
</script>