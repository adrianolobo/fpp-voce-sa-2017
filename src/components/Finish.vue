<template>
  <div class="finish modal">
    <div class="modal__title">
      Parabéns!
    </div>
    <div class="modal__content">
      <img src="/static/img/finish2.jpg" alt="">
    </div>
    <div class="modal__footer">
      <button 
        class="button"
        @click="restartGame()"
      >
        Jogar novamente
      </button>
    </div>
  </div>
</template>
<style lang="sass" scoped>
  .finish

</style>

<script>

export default {
  components: {
  },
  props: [],
  data() {
    return {
    };
  },
  methods: {
    restartGame() {
      this.$emit('restart-game');
    },
  },
  computed: {
  },
  created() {
  },
};
</script>
