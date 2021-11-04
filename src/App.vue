<template>
  <Header />
  <div class="board">
    <div class="carril">
      <h2>Todo</h2>
      <Container
        group-name="trello"
        @drag-start="handleDragStart('todo', $event)"
        @drop="handleDrop('todo', $event)"
        :get-child-payload="getChildPayload"
      >
        <Draggable v-for="card in cards.todo" :key="card.id">
          <Card>
            {{ card.text }}
          </Card>
        </Draggable>
      </Container>
    </div>
    <div class="carril">
      <h2>Doing</h2>
      <Container
        group-name="trello"
        @drag-start="handleDragStart('doing', $event)"
        @drop="handleDrop('doing', $event)"
        :get-child-payload="getChildPayload"
      >
        <Draggable v-for="card in cards.doing" :key="card.id">
          <Card> {{ card.text }} </Card>
        </Draggable>
      </Container>
    </div>
    <div class="carril">
      <h2>Done</h2>
      <Container
        group-name="trello"
        @drag-start="handleDragStart('done', $event)"
        @drop="handleDrop('done', $event)"
        :get-child-payload="getChildPayload"
      >
        <Draggable v-for="card in cards.done" :key="card.id">
          <Card>
            {{ card.text }}
          </Card>
        </Draggable>
      </Container>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import Header from "./components/Header.vue";
import Card from "./components/Card.vue";
import initialCards from "./helpers/initialCards.js";
import { Container, Draggable } from "vue3-smooth-dnd";

export default {
  setup() {
    const cards = ref({
      todo: ref(initialCards.todo),
      doing: ref(initialCards.doing),
      done: ref(initialCards.done),
    });

    const draggingCard = ref({
      carril: '',
      index: -1,
      cardData: {}
    });

    const handleDragStart = (carril, dragResult) => {
      const { payload, isSource } = dragResult;
      console.log(  )
      if (isSource) {
        draggingCard.value = {
          carril: carril,
          index: payload,
          cardData: {
            ...cards.value[carril][payload]
          }
        }
      }
        
      console.log(payload);
      console.log( dragResult );
    };
    const handleDrop = (carril, dropResult) => {
      const { removedIndex, addedIndex } = dropResult;

      if (carril == draggingCard.value.carril && removedIndex == addedIndex){
        return;
      }

      if (removedIndex !== null) {
        cards.value[carril].splice(removedIndex, 1);
      }
      if (addedIndex !== null) {
        cards.value[carril].splice(addedIndex, 0, draggingCard.value.cardData);
      }

    };
    const getChildPayload = (index) =>{
      return(index );
    };

    return {
      cards,
      handleDrop,
      handleDragStart,
      getChildPayload
    };
  },
  components: {
    Header,
    Card,
    Container,
    Draggable,
  },
};
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
}

body {
  background: rgb(30, 230, 170);
  font-family: Helvetica, sans-serif;
}

.board {
  display: flex;
  justify-content: flex-start;
  .carril {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    flex-direction: column;
    background: rgb(230, 230, 210);
    width: 20rem;
    min-height: 20rem;
    margin-left: 1rem;
    margin-top: 1rem;
    border-radius: 0.3rem;
    h2 {
      padding: 0.5rem 0 0.5rem 0;
      text-align: center;
      width: 100%;
      color: white;
      border-radius: 0.3rem 0.3rem 0 0;
      background: none;
      background: rgba(50, 100, 170, 0.6);
    }
  }
}
</style>
