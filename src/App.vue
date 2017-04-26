<template>
  <div id="app">
    <div class="app__container" v-if="suportedBrowser">
      <div class="game" :class="{ 'game_modal': isModalOpen }">
        <div class="cards">
          <card 
            v-for="card in cards"
            :card="card"
            class="cards__card"
            v-on:card-selectd="selectCard(card)"
          ></card>
        </div>
      </div>
      <transition name="fade">
        <start 
          v-if="stage === 'start'"
          v-on:start-game="startGame()"
        ></start>
      </transition>
      <transition name="fade">
        <finish 
          v-if="stage === 'finish'"
          v-on:restart-game="restartGame()"
        ></finish>
      </transition>
    </div>
    <div v-if="!suportedBrowser">
      <p>
        Browser não suportado. <br>
        Utilize Chrome, Firefox ou Edge.
      </p>
    </div>
  </div>
</template>
<style src="./styles.sass" lang="sass"></style>
<script>
import _ from 'lodash';
import is from 'is_js';
import Card from './components/Card';
import cards from './components/Cards.json';
import Start from './components/Start';
import Finish from './components/Finish';

export default {
  name: 'app',
  components: {
    Card,
    Start,
    Finish,
  },
  data() {
    return {
      cards,
      isLoading: true,
      stage: 'start',
    };
  },
  methods: {
    startGame() {
      this.stage = 'game';
      this.isLoading = true;
      setTimeout(() => {
        this.cards = _.shuffle(cards);
        this.setSelectedCards(true);
        setTimeout(() => {
          this.setSelectedCards(false);
          this.isLoading = false;
        }, 10000);
      }, 1000);
    },
    resetGame() {
      for (let i = this.cards.length - 1; i >= 0; i -= 1) {
        this.cards[i].is_selected = false;
        this.cards[i].matched = false;
      }
    },
    restartGame() {
      this.resetGame();
      this.startGame();
    },
    setSelectedCards(selected) {
      for (let i = this.cards.length - 1; i >= 0; i -= 1) {
        this.cards[i].is_selected = selected;
      }
    },
    selectCard(card) {
      if (this.isLoading) {
        return;
      }
      let amountSelectedTurn = 0;
      let amountMatched = 0;
      let selectedCard = 0;
      for (let i = this.cards.length - 1; i >= 0; i -= 1) {
        if (this.cards[i].is_selected) {
          if (!this.cards[i].matched) {
            selectedCard = this.cards[i];
            amountSelectedTurn += 1;
          } else {
            amountMatched += 1;
          }
        }
      }
      const thisCard = card;
      if (amountSelectedTurn === 1 || amountSelectedTurn === 0) {
        thisCard.is_selected = true;
      }
      if (amountSelectedTurn === 1) {
        if (selectedCard.id === thisCard.id) {
          selectedCard.matched = true;
          thisCard.matched = true;
          if (amountMatched === (this.cards.length - 2)) {
            this.isLoading = true;
            setTimeout(() => {
              this.stage = 'finish';
            }, 1000);
          }
        } else {
          this.isLoading = true;
          setTimeout(() => {
            selectedCard.is_selected = false;
            thisCard.is_selected = false;
            this.isLoading = false;
          }, 1000);
        }
      }
    },
  },
  computed: {
    isModalOpen() {
      return (this.stage === 'start' || this.stage === 'finish');
    },
    suportedBrowser() {
      if (is.safari() || is.ie()) {
        return false;
      }
      return true;
    },
  },
  created() {
  },
  mounted() {
  },
};
</script>

<style lang="sass">
@import url(https://fonts.googleapis.com/css?family=Roboto+Slab)

html,body
  height: 100%
  font-family: Roboto Slab
  background-color: #027697
.game
  transition: filter .3s ease
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center
  &_modal
    filter: blur(10px)
.cards
  display: flex
  flex-wrap: wrap
  justify-content: center
  align-items: center
  margin-top: 10px
  width: 900px
  &__card
    margin: 10px
.fade-enter-active, .fade-leave-active
  transition: opacity .3s
.fade-enter, .fade-leave-to
  opacity: 0
</style>
