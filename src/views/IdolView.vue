<template lang="pug">
  .idol-view-container
    
    //- search
    .idol-card.glass
      .input-wrapper
        input(type="text" v-model="searchTerm" @keyup.enter="handleSearch" placeholder="Search idol")
        .search-input.pi.pi-search(@click="handleSearch")
    
    //- idol card
    .idol-card.glass(v-for="idol in filteredIdol" :key="idol.id")
      .idol-card__img-container
        img.idol-card__img.glitch-img(v-if="idol && idol.thumb_url" :src="idol.thumb_url" alt="Idol Image" draggable="false")
      .idol-card__info
        span.stage-title {{ idol.name_original }}
        span.idol-name.baffle  | {{ idol.name }}
        .idol-name.baffle Real Name: {{ idol.real_name }} |  {{ idol.real_name_original }}
        .idol__.baffle Height: {{ idol.height }}cm
        .idol__.baffle(v-if="idol.weight") Weight: {{ idol.weight }}kg
        .idol__.baffle Date of Debut: {{ formattedDate(idol.debut_date) }}
        .idol__.baffle Date of Birth: {{ formattedDate(idol.birth_date) }}
    .idol-card.glass(v-for="group in filteredGroup" :key="group.id")
      img.idol-card__img.glitch-img(v-if="group && group.thumb_url" :src="group.thumb_url" alt="GRoup Image" draggable="false")
      //- {{ idol.real_name_original }}
</template>

<script lang="ts">
import kpopData from 'kpopnet.json';
import type { Idol, Group } from '@/components/types';

import $ from 'jquery';
import moment from 'moment';
import baffle from 'baffle';

// https://github.com/kpopnet/kpopnet.json/blob/master/kpopnet.d.ts
// https://unpkg.com/kpopnet.json@2.0.5/kpopnet.json
// https://selca.kastden.org/noona/idol/aespa_winter/

export default {
  data() {
    return {
      searchTerm: 'Winter',
      relatedGroupIds: [] as string[],
      kpopData: kpopData as { idols: Idol[]; groups: Group[] },
    };
  },
  mounted () {
    this.startBaffle();
    this.startGlitch();
  },
  computed: {
    filteredIdol(): Idol[] {
      // Ensure idols exist before filtering
      if (!this.kpopData.idols) return [];
      // console.log(this.kpopData);
      let res = this.kpopData.idols.filter((idol) =>
        idol.name.toLowerCase() === this.searchTerm.toLowerCase()
      );
      this.relatedGroupIds = res[0].groups;
      return res;
    },
    filteredGroup(): Group[] | void {
      // if (!this.relatedGroupIds.groups) return [];
      // // console.log(this.relatedGroupIds);

      // // TODO loop group array, into group id
      // return this.relatedGroupIds.groups.filter((group) => group.id === '7WJQQrEe45xdB9XpC5FY');
    },
  },
  methods: {
    formattedDate(date : string) {
      return moment(date).format('Do MMMM YYYY'); 
    },
    handleSearch() {
      // Handle both button click and Enter key
      this.startBaffle();
      this.startGlitch();
    },

    // start the obfuscate
    startBaffle() {
      let b = baffle('.baffle', {
      characters: '░▒█▓▓▒█/▒',
      speed: 80
      });
      b.reveal(5000);
    },

    // start the obfuscate
    startGlitch() {
      $( ".glitch-img" ).mgGlitch({
        // set 'true' to stop the plugin
        destroy : false, 
        // set 'false' to stop glitching
        glitch: true, 
        // set 'false' to stop scaling
        scale: true, 
        // set 'false' to stop glitch blending
        blend : true, 
        // select blend mode type
        blendModeType : 'hue',
        // set min time for glitch 1 elem
        glitch1TimeMin : 10, 
        // set max time for glitch 1 elem
        glitch1TimeMax : 100,
        // set min time for glitch 2 elem
        glitch2TimeMin : 10, 
        // set max time for glitch 2 elem
        glitch2TimeMax : 300, 
      });
    }
  }
};
</script>

<style lang="scss">
.idol-card {
  display: flex;
  flex-direction: row;
  gap: 20px;
  margin: 20px auto;
  background-color: aqua;
  padding: 12px;

  @media (max-width: 768px) {
    flex-direction: column;
  }

  &.glass{
    background: linear-gradient(135deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    border-radius: 20px;
    border:1px solid rgba(255, 255, 255, 0.18);
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
  }

  &__img {
    border-radius: 30px;
    user-select: none;
    -o-user-select: none;
    -moz-user-select: none;
    -khtml-user-select: none;
    -webkit-user-select: none;
    
    &-container {
      position: relative;
      height: 300px;
      aspect-ratio: 1/1;

      @media (max-width: 768px) {
        height: 100%;
      }
    }
  }

  &__info {
    .stage-title {
      font-family: "Do Hyeon", sans-serif;
      font-size: 64px;
    }
  }

  .input-wrapper {
    position: relative;
    
    input {
      /* From Uiverse.io by Praashoo7 */ 
      font-family: "Do Hyeon", sans-serif;
      border: none;
      outline: none;
      border-radius: 15px;
      padding: 1em;
      letter-spacing: 1.8px;
      color: var(--color-text);
      background-color: black;
      transition: 300ms ease-in-out;
    }
    .search-input {
      position: absolute;
      right: 0;
      top: 50%;
      transform: translate(-50%, -50%);
      cursor: pointer;
    }
  }
}

/* glitch elem must have absolute position */
.glitch-img {
    position: absolute; 
    width : 100%;
    height : 100%; 
    top: 0 ; 
    left : 0; 
    opacity : 1;
    
    background-position:center;  
    -moz-background-size:cover;
    -o-background-size:cover;
    -webkit-background-size:cover;
    background-size:cover;
  } 
</style>