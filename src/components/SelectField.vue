<template>
  <div>
    <Label 
    :id="id"
    :label="label"
    />
    <Select 
    :options="options"
    :name="name"
    :required="required"
    :modelValue="modelValue"
    @set-select="setSelect"
    />
  </div>
</template>

<script>
import Label from "./Label.vue"
import Select from "./Select.vue"

export default {
  props: ["id", "label", "options", "name", "required", "modelValue"],
  components: {
    Label,
    Select
  },
  methods: {
    setSelect(selectedOption) {
      this.$emit("update:modelValue", selectedOption)
    }
  }
}

</script>