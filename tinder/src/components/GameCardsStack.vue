<template>
  <div class="main">
    <GameCard />
  </div>
</template>
<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import GameCard from "@/components/GameCard.vue";
@Component({
  components: {
    GameCard
  }
})
export default class GameCardsStack extends Vue {}
</script>