<script>
import Card from './components/Card.vue'
import { ref, watch } from 'vue'
export default {
  name: 'App',
  components: {
    Card
  },
  setup () {
    const cardList = ref([])
    const userSelection = ref([])
    const status = ref('')

    for (let i = 0; i <16; i++) {
      cardList.value.push({
        value: 10,
        visible: false,
        position: i
      
      })
    }

    const flipCard = payload => {
      cardList.value[payload.position].visible = !cardList.value[payload.position].visible

      if(userSelection.value[0]) {
        userSelection.value[1] = payload
      }
      else {
        userSelection.value[0] = payload
      }
    }

    watch(userSelection, currentValue => {
      if(currentValue.length === 2) {
        const cardOne = currentValue[0]
        const cardTwo = currentValue[1]

        if(cardOne.faceValue === cardTwo.faceValue) {
          status.value = 'Match Found'
        } else {
          status.value = 'No Match Found'
          cardList.value[cardOne.position].visible = false
          cardList.value[cardTwo.position].visible = false
        }

        
          userSelection.value.length = 0
      } 
    },
    {deep: true})

    return {
      cardList,
      flipCard,
      userSelection,
      status
    }
  }
}
</script>

<template>
  <h1>Memory Card</h1>
  <section class="game-board">
  <Card v-for="(card,index) in cardList" :key="`card-${index}`" :value="card.value" :visible="card.visible" @select-card="flipCard" :position="card.position"/>
  </section>
  <h2>{{status}}</h2>
</template>

<style scoped>

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}


.game-board {
  margin: 0 auto;
  display: grid;
  justify-content: center;
  grid-template-columns: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  grid-template-rows: 100px 100px 100px 100px;
}

</style>
