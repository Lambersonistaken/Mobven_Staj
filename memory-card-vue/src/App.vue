<script>
import Card from './components/Card.vue'
import { ref, watch, computed } from 'vue'
import _ from 'lodash'
export default {
  name: 'App',
  components: {
    Card
  },
  setup () {
    const cardList = ref([])
    const userSelection = ref([])
    const status = computed(() => {
     if(remainingPairs.value === 0){
      return "You Win!"
     }
      else {
        return "Keep Playing, You have " + remainingPairs.value + " pairs left to match."
      }
    })


    const remainingPairs = computed(() => {
      const remainingCards = cardList.value.filter(card => card.matched === false)

      return remainingCards.length / 2
    })

   


    const restartGame = () => {
      cardList.value = _.shuffle(cardList.value) // lodash kullanarak shuffle işlemi yaptık.

      cardList.value = cardList.value.map((card,index) => {
        return {
          ...card,
          visible: false,
          matched: false,
          position: index
        }
      })
    }

    const cardItems = ["celtics","dallas","denver","ers","lakers","miami","knicks","golden"]


    cardItems.forEach( item => {
      cardList.value.push({
        value: item,
        variant:1,
        visible: false,
        position: null,
        matched: false
      })
      cardList.value.push({
        value: item,
        variant:2,
        visible: false,
        position: null,
        matched: false
      })
    })

    cardList.value = cardList.value.map((card,index) => {
      return {
        ...card,
        position: index
      }
    })

    

    const flipCard = payload => {
      cardList.value[payload.position].visible = !cardList.value[payload.position].visible

      if(userSelection.value[0]) {
        if(
          userSelection.value[0].position === payload.position
          && userSelection.value[0].faceValue === payload.faceValue
        ) {
          return
        }
        else {
          userSelection.value[1] = payload
        }
        
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
          
          cardList.value[cardOne.position].matched = true
          cardList.value[cardTwo.position].matched = true
        } else {
          setTimeout(() => {
            cardList.value[cardOne.position].visible = false
            cardList.value[cardTwo.position].visible = false
          }, 1000)
        }

        
          userSelection.value.length = 0
      } 
    },
    {deep: true})

    return {
      cardList,
      flipCard,
      userSelection,
      status,
      restartGame
    }
  }
}
</script>

<template>
  <h1>Memory Card</h1>
  <transition-group class="game-board" tag="section" name="shuffle-card">
  <Card v-for="(card) in cardList" :key="`${card.value}-${card.variant}`" :value="card.value" :matched="card.matched" :visible="card.visible" @select-card="flipCard" :position="card.position"/>
  </transition-group>
  <h2>{{status}}</h2>
  <button @click="restartGame">RESTART</button>
</template>

<style scoped>

.game-board {
  margin: 0 auto;
  display: grid;
  justify-content: center;
  grid-template-columns: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  grid-template-rows: 100px 100px 100px 100px;
}

.shuffle-card-move {
  transition: transform 0.8s ease-in;
}

</style>
