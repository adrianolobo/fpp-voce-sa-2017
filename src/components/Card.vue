<template>
  <div class="card" @click="selectCard()" 
    :class="{ 'card_flipped' : isSelected, 'card_matched' : isMatched,}">
    <div class="card__back">
      <div class="card__back-container">
        <div class="card__fpp">
          <span>
            FPP
          </span>
        </div>
      </div>
    </div>
    <div class="card__front">
      <div 
        class="card__front_image"
        :style="{ backgroundImage: `url(/static/img/${card.file_name})` }"
      ></div>
    </div>
  </div>
</template>
<style lang="sass" scoped>
  .card
    perspective: 800px
    background-color: white
    height: 200px
    width: 120px
    border-radius: 5px
    box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24)
    transition: all 1s cubic-bezier(.25,.8,.25,1)
    display: flex
    justify-content: center
    align-items: center
    transform-style: preserve-3d
    cursor: pointer
    &:hover
      box-shadow: 0 14px 28px rgba(0,0,0,0.25), 0 10px 10px rgba(0,0,0,0.22)
    &_flipped
      transform: rotateY( 180deg )
    &_matched
      box-shadow: 0 10px 10px rgba(0,180,54,0.7) !important
    &__back-container
      width: 90%
      height: 90%
      border-radius: 5px
      background-color: #0abced
      align-items: center
      display: flex
      justify-content: center
    &__back
      position: absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
      display: flex
      justify-content: center
      align-items: center
      backface-visibility: hidden
    &__front
      border-radius: 5px
      position: absolute
      top: 0
      left: 0
      backface-visibility: hidden
      height: 100%
      width: 100%
      background-color: #0abced
      transform: rotateY( 180deg )
      &_image
        background-size: contain
        background-position: center center
        width: 80%
        height: 60%
        background-repeat: no-repeat
        position: absolute
        top: 50%
        left: 50%
        transform: translate(-50%, -50%)
    &__fpp
      height: 80px
      width: 80px
      border-radius: 50%
      background-color: #fff
      position: relative
      display: flex
      justify-content: center
      align-items: center
      span
        color: #0abced
        font-size: 28px
        position: relative
        font-weight: bold

</style>

<script>

export default {
  components: {
  },
  props: ['card'],
  data() {
    return {
    };
  },
  methods: {
    selectCard() {
      this.$emit('card-selectd', this.card);
    },
  },
  computed: {
    isSelected() {
      return this.card.is_selected;
    },
    isMatched() {
      return this.card.matched;
    },
  },
  created() {
  },
};
</script>
