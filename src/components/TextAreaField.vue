<template>
  <div>
    <Label 
    :id="id"
    :label="label"
    />
    <TextArea 
    :id="id"
    :name="name"
    :required="required"
    :modelValue="modelValue"
    @set-textarea="setTextArea"
    />
  </div>
</template>

<script>
import Label from "./Label.vue"
import TextArea from "./TextArea.vue"

export default {
  components: {
    Label,
    TextArea 
  },
  props: ["id", "label", "name", "required", "modelValue"],
  methods: {
    setTextArea(text) {
      this.$emit("update:modelValue", text)
    }
  }
}
</script>