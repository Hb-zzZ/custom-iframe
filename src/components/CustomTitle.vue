<template>
  <h2
    style="border-left: 3px solid rosybrown;
  padding-left: 16px;"
  >
    {{ title }}
  </h2>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: ''
    }
  }
}
</script>
