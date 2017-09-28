<template>
  <div id="app">
    <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
    show: true
    }
  }
}
</script>

<style>

.fade-enter-active {
  animation: boun2ce2-in .5s
}
.fade-leave-active {
  animation: boun2ce2-in .5s reverse;
}

@keyframes boun2ce2-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

// .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
//   opacity: 0
// }

</style>
