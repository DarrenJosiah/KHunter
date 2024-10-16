<template lang="pug">
    .home-view-container
        .home-view__img-container(v-for="(idol, index) in visibleIdols" :key="idol.id")
            FlipCard(
                :img="idol.thumb_url"
                :stageNameKr="idol.name_original"
                :stageNameEng="idol.name"
            )
                //- img.home-view__img.glitch-img(v-if="idol && idol.thumb_url" :src="idol.thumb_url" alt="Idol Image" draggable="false")
</template>
  
<script lang="ts">
import kpopData from 'kpopnet.json';
import type { Idol, Group } from '@/components/types';

// @ts-ignore
import FlipCard from './atoms/FlipCard.vue';

import $ from 'jquery';

export default {
    components: {
        FlipCard,
    },
    data() {
        return {
            searchTerm: '',
            relatedGroupIds: [] as string[],
            kpopData: kpopData as { idols: Idol[]; groups: Group[] },
            visibleIdols: [] as Idol[], // Stores idols to show
            itemsPerPage: 10, // Items loaded per scroll
            page: 1, // Current page index
        };
    },
    mounted () {
        this.loadMore();
        this.startGlitch();
        window.addEventListener('scroll', this.handleScroll);
    },
    beforeDestroy() {
        window.removeEventListener('scroll', this.handleScroll);
    },
    computed: {
        filteredIdol(): Idol[] {
            if (!this.kpopData.idols) return [];
            let res = this.kpopData.idols.filter(idol =>
                idol.name.toLowerCase().includes(this.searchTerm.toLowerCase())
            );
            this.relatedGroupIds = res[0]?.groups || [];
            res = this.shuffleArray(res);
            return res;
        },
    },
    methods: {
        loadMore() {
            const start = (this.page - 1) * this.itemsPerPage;
            const end = this.page * this.itemsPerPage;
            this.visibleIdols = [
                ...this.visibleIdols,
                ...this.filteredIdol.slice(start, end),
            ];
            this.page++;
        },
        handleScroll() {
            const bottomOfWindow =
                window.innerHeight + window.scrollY >= document.body.offsetHeight - 10;
            if (bottomOfWindow) {
                this.loadMore();
            }
        },
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
        },
        // Fisher-Yates Shuffle Algorithm
        shuffleArray(array: Idol[]): Idol[] {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]]; // Swap elements
            }
            return array;
        }
    }
};
</script>
  
<style lang="scss">
.home-view-container {
    display: flex;
    gap: 20px;
    overflow: scroll;
    height: 400px;
}

.home-view {
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
        }
    }

    &__info {
        .stage-title {
        font-family: "Do Hyeon", sans-serif;
        font-size: 64px;
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