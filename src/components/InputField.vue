<template>
  <div>
    <Label 
    :label="label"
    :id="id"
    />
    <Input 
    :id="id"
    :type="type"
    :name="name"
    :modelValue="modelValue"
    @set-input="setInput"
    :required="required"
    />
  </div>
</template>

<script>
import Input from "./Input.vue"
import Label from "./Label.vue"

export default {
  props: ["label", "id", "type", "name", "modelValue", "required"],
  components: {
    Input,
    Label
  },
  methods: {
    setInput(input) {
      this.$emit("update:modelValue", input)
    }
  }

}

</script>
