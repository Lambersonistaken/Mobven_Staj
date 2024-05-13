<template>
  <div class="card" :class="flippedStyle" @click="selectCard">
    <div class="card-face is-front">
      <img :src="`/images/${value}.png`" :alt="value">
    </div>
    <div  class="card-face is-back">
    </div>
  </div>
</template>

<script>

import { computed } from 'vue'
    export default {
        props:{
          matched: {
            type: Boolean,
            default: false
          },
            position: {
                type: Number,
                required: true
            },
            value: {
                type: String,
                required: true
            },
            visible: {
                type: Boolean,
                default: false
            }
        },
        setup(props,context){
            const flippedStyle = computed(() => {
                if(props.visible){
                    return "is-flipped"
                }
            })


            const selectCard = () => {
                context.emit('select-card',{
                    position: props.position,
                    faceValue: props.value
                })
            }
            return {
                selectCard,
                flippedStyle
            }
        }
        
    }
</script>

<style lang="css" scoped>
.card {
  position: relative;
  transition: 0.5s transform ease-in;
  transform-style: preserve-3d;
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  border-radius: 4px;
  border: 2px solid #000000fd;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  backface-visibility: hidden;
}

.card-face img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 4px;
}

.card.is-flipped {
  transform: rotateY(180deg);
}

.card-face.is-front {
  background-color: #f81559  ;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  transform: rotateY(180deg);
}
.card-face.is-back {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #2b7bf5;
  color: white;
}
</style>