<template>
  <StarryBackground @rotation-change="updateRotation" />
  <div 
    class="content-container"
    :style="{
      transform: `perspective(1000px) 
                 rotateX(${rotation.x}deg) 
                 rotateY(${rotation.y}deg)`
    }"
  >
    <AnimatedTitle />
    <GlassButton :visible="showButton" />
  </div>
</template>

<script>
import StarryBackground from './components/StarryBackground.vue'
import AnimatedTitle from './components/AnimatedTitle.vue'
import GlassButton from './components/GlassButton.vue'

export default {
  name: 'App',
  components: {
    StarryBackground,
    AnimatedTitle,
    GlassButton
  },
  data() {
    return {
      showButton: false,
      rotation: {
        x: 0,
        y: 0
      }
    }
  },
  mounted() {
    // Show button after 4 seconds (after title animation completes)
    setTimeout(() => {
      this.showButton = true
    }, 4000)
  },
  methods: {
    updateRotation(newRotation) {
      this.rotation = newRotation
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  position: relative;
  z-index: 1;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 6rem;
}

.content-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 6rem;
  transform-style: preserve-3d;
  transform-origin: center center;
  will-change: transform;
}
</style>
