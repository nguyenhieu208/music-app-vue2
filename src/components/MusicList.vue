<template>
  <button
    @click="play(song)"
  >
    {{ song.title }} - {{ song.artist }}
  </button>
</template>

<script>
export default {
  props: ['song'],
  data() {
    return {
      
    }
  },
  methods: {
    play(song) {
      this.$emit('play', song);
    }
  }
}
</script>

<style scoped>

</style>