<template>
  <div class="container">
    <section class="draggable-container">
      <GameCardsStack />
      <!-- <GameCard v-for="(card, index) in people" :key="index" /> -->
    </section>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";
import axios from "axios";
import GameCard from "@/components/GameCard.vue";
import GameCardsStack from "@/components/GameCardsStack.vue";

@Component({
  components: { GameCard, GameCardsStack }
})
export default class App extends Vue {
  mounted() {
    axios
      .get(
        `https://randomuser.me/api/?results=5&gender=female`
      )
      .then(response => (this.people = response.data))
  }

  cards = [1];
  people = [];
}
</script>

<style lang="scss" scoped>
@import "./styles/index.scss";

$cardsWidth: 200px;
$fs-card-title: 1.125em;

.container {
  margin-top: 100px;
}

.draggable-container {
  @include sizing(200px);

  display: flex;
  margin: auto auto 100px;
}
</style>
