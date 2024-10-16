<template lang="pug">
  .title
    h1.titleBaffle(v-on:mouseover="startBaffle") K-Hunter
</template>

<script lang="ts">
import baffle from 'baffle';

export default {
  mounted () {
    this.startBaffle();
  },
  methods: {
    // start the obfuscate
    startBaffle() {
      let b = baffle('.titleBaffle', {
      characters: '케-사냥꾼',
      speed: 80
      });
      b.reveal(5000);
    },
  }
};
</script>

<style scoped lang="scss">
.title {
  text-align: end;
  font-family: "Do Hyeon", sans-serif;
  font-weight: 300;
  font-size: 2rem;

  @media (max-width: 768px) {
    font-size: 1.5rem;
  }
}
</style>
