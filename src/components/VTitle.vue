<template>
  <component :is="tag" class="v-title">
    <slot />
  </component>
</template>

<script>
export default {
  props: {
    tag: {
      type: String,
      default: 'h1'
    }
  }
}
</script>

<style lang="scss" scoped>
.v-title {
  font-size: 48px;
}
</style>
