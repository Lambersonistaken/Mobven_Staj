<template>
  <div class="card" @click="selectCard">
    <div v-if="visible" class="card-face is-front">
      <img :src="`/images/${value}.png`" :alt="value">
    </div>
    <div v-else class="card-face is-back">
      Back
    </div>
  </div>
</template>

<script>
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
            const selectCard = () => {
                context.emit('select-card',{
                    position: props.position,
                    faceValue: props.value
                })
            }
            return {
                selectCard
            }
        }
        
    }
</script>

<style lang="css" scoped>
.card {
  
  position: relative;
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
}

.card-face img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 4px;
}

.card-face.is-front {
  background-color: #f81559  ;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
}
.card-face.is-back {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #2b7bf5;
  color: white;
}
</style>