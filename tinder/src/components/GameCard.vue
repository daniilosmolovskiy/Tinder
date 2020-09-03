<template>
  <div v-if="haveData">
    <Vue2InteractDraggable
      v-for="(card, index) in people"
      :key="index"
      @draggedDown="draggedDown(index)"
      @draggedLeft="draggedLeft(index)"
      @draggedRight="draggedRight(index)"
      @draggedUp="draggedUp(index)"
      :interact-max-rotation="15"
      :interact-out-of-sight-x-coordinate="500"
      :interact-x-threshold="200"
      class="card isCurrent"
      :class="card.draggedClass"
    >
      <div>
        <img :src="card.picture.thumbnail" />
        <h3 class="cardTitle">{{card.name.first}}</h3>
        <p class="age">{{card.dob.age}}</p>
        <p class="age">
          {{card.location.country}},
          <br />
          {{card.location.city}}
        </p>
      </div>
    </Vue2InteractDraggable>
    <div class="test" v-if="done">
      <h1>FINISH</h1>
    </div>
  </div>
</template>
<script>
import { Vue2InteractDraggable } from "vue2-interact";
import { Component, Vue } from "vue-property-decorator";
import axios from "axios";
@Component({
  components: { Vue2InteractDraggable }
})
export default class GameCard extends Vue {
  mounted() {
    axios
      .get(
        `https://randomuser.me/api/?inc=name,location,dob,picture&results=5&gender=female`
      )
      .then(
        response =>
          (this.people = response.data.results.map(person => {
            person.draggedClass = null;
            return person;
          }))
      )
      .then((this.haveData = true));
  }
  people = [];
  haveData = false;
  counter = 0;
  isCurrent = true;
  isShowing = true;
  done = false;
  v1 = [];
  v2 = [];
  v3 = [];
  draggedDown(index) {
    console.log("dragged down!", index);
    this.people[index].draggedClass = "dragged-down";
    this.hideCard();
  }
  draggedLeft(index) {
    console.log("dragged left!", index);
    this.people[index].draggedClass = "dragged-left";
    this.hideCard();
  }
  draggedRight(index) {
    console.log("dragged right!", index);
    this.people[index].draggedClass = "dragged-right";
    this.hideCard();
  }
  draggedUp(index) {
    console.log("dragged up!", index);
    this.people[index].draggedClass = "dragged-up";
    this.hideCard();
  }
  hideCard() {
    setTimeout(() => {
      this.isShowing = false;
    }, 200);
    setTimeout(() => {
      this.isShowing = true;
    }, 1000);
    this.counter++;
    if (this.counter === this.people.length - 1) {
      setTimeout(() => {
        this.done = true;
      }, 2000);
    }
  }
}
</script>


<style lang="scss" scoped>
@import "../styles/index.scss";

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

.switches {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;

  &__article {
    margin-right: 30px;
  }
}

.property-switch {
  margin-top: 5px;
}

.card {
  @include card();
  @include absolute(0);
  @include sizing(200px);
  @include flex-center();

  display: flex;
  max-height: 200px;
  margin: auto;
  font-size: $fs-h2;
  font-weight: $fw-bold;
  color: $c-white;
  background-image: linear-gradient(
    -180deg,
    $primary-gradient-start 2%,
    $primary-gradient-end 100%
  );
  opacity: 1;
  transform-origin: 50%, 100%;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  user-select: none;
  pointer-events: none;
  will-change: transform, opacity;

  height: 100vw;

  &.isCurrent {
    pointer-events: auto;
  }

  &.isAnimating {
    transition: transform 0.7s $ease-out-back;
  }
}

.dragged-left {
  // position: relative;
  &:before {
    content: '';
    display: block;
    position: absolute;
    width: 100%;
    height: 100%;
    background: url('../assets/cheap.png') no-repeat;
    background-size: contain;
  }
}

.cardTitle {
  margin: 0 0 15px;
  font-size: $fs-card-title;
}
</style>