<template>
  <div v-for="error of errors" :key="error.$uid">
    <div>{{ error.$message }}</div>
  </div>
</template>
<script>
export default {
  name: 'IngVuelidateFieldErrors',
  props: {
    errors: {
      required: true
    },
  },
  setup (props) {
    return {
      props
    }
  }
}
</script>

