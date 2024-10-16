<template lang="pug">
  .flip-card
    .content
      .front
        img.home-card__img.glitch-img(:src="img" alt="Idol Image" draggable="false")
      .back
        .back-title {{ stageNameKr }}
        .back-title {{ stageNameEng }}

</template>

<script setup>
defineProps(['img','stageNameKr', 'stageNameEng'])
</script>

<style lang="scss">
.flip-card {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 300px;
  height: 300px;
  margin: -150px;
  float: left;
  perspective: 500px;

  &:hover .content {
  transform: rotateY( -180deg ) ;
  transition: transform 0.5s;
  }
  
  .content {
    position: absolute;
    width: 100%;
    height: 100%;
    box-shadow: 0 0 15px rgba(0,0,0,0.1);

    transition: transform 1s;
    transform-style: preserve-3d;
  }

  .front,
  .back {
    position: absolute;
    height: 100%;
    width: 100%;
    background: white;
    // line-height: 300px;
    color: #03446A;
    text-align: center;
    font-size: 60px;
    border-radius: 5px;
    backface-visibility: hidden;
  }

  .back {
    background: #03446A;
    color: white;
    transform: rotateY( 180deg );
    
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
}
</style>