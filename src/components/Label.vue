<template>
  <label 
  :for="id"
  class="block text-sm font-medium text-gray-900"
  >
    {{ label }}
  </label>
</template>

<script>
export default {
  props: ["id", "label"]
}
</script>