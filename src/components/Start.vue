<template>
  <div class="start modal">
    <div class="modal__title">
      Jogo da memória - FPP 100% Você S/A
    </div>
    <div class="modal__content"></div>
    <div class="modal__footer">
      <button 
        class="button"
        @click="startGame()"
      >
        Iniciar jogo
      </button>
    </div>
  </div>
</template>
<style lang="sass" scoped>
  .start

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
    startGame() {
      this.$emit('start-game');
    },
  },
  computed: {
  },
  created() {
  },
};
</script>
