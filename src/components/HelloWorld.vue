<template lang="pug">
  .title
    h1.titleBaffle K-Hunter
</template>

<script lang="ts">
import baffle from 'baffle';

// https://github.com/kpopnet/kpopnet.json/blob/master/kpopnet.d.ts
// https://unpkg.com/kpopnet.json@2.0.5/kpopnet.json
// https://selca.kastden.org/noona/idol/aespa_winter/

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

<style scoped>
.title {
  text-align: end;
  font-family: "Do Hyeon", sans-serif;
  font-weight: 300;
  font-size: 2rem;
}
</style>
