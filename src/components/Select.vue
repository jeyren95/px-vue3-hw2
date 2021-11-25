<template>
  <div class="mt-1">
    <select
    class="block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-pink-500 focus:border-pink-500 sm:text-sm rounded-md"
    :id="id"
    :name="name"
    :required="required"  
    :value="modelValue"
    @change="handleChange"
    >
      <option 
      v-for="(option, index) in options" 
      :key="index"
      :value="option.value"
      >
        {{ option.display }}
      </option>
    </select>
  </div>

</template>

<script>
export default {
  props: ["id", "name", "required", "options", "modelValue"],
  methods: {
    handleChange(e) {
      this.$emit("set-select", e.target.value)
    }
  }
}
</script>