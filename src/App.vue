<template>
  <div id="app">
    <div class="preview__block gradient"
         :class="{ hide }">
      <button class="btn big-btn transparent"
              @click="showPopup">
        Налоговый вычет
      </button>
    </div>
    <Popup :class="{ 'hide':!hide }" @close="close"/>
  </div>
</template>

<script>
import Popup from "./components/Popup.vue"
export default {
  name: 'App',
  data: () => ({
    hide: true
  }),

  methods: {
    showPopup() {
      this.hide = true
    },
    close() {
      this.hide = false
    }
  },

  components: {
    Popup
  }
}
</script>

<style lang="scss">
  .preview__block {
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: .3s;
    .btn {
      filter: drop-shadow(0px 0px 44px #CFDAE7);
    }
  }
</style>
