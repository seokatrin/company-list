<template>
  <div>
    <label class="lebel">{{ title }}
        <input  :value="modelValue" @input="updateInput" class="input" />
    </label>
  </div>
</template>
<script>
export default {
  name: 'form-input',
  props: {
    title: String,
    modelValue: [String, Number]
  },
  methods: {
    updateInput (e) {
      this.$emit('update:modelValue', e.target.value)
    }
  }
}
</script>
<style lang="scss" scoped>

</style>
