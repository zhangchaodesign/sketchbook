<template>
  <div @click="handleClick">
    <img src='https://raw.githubusercontent.com/chaochaooo/Bio_Sketchbook/main/public/assets/svg/next.svg'>
  </div>
</template>

<script>

export default {
  methods: {
    handleClick(e) {
      this.$router.push({ path: '/plant' })
      this.$emit('click', e)
    }
  }
}
</script>

<style scoped>
</style>