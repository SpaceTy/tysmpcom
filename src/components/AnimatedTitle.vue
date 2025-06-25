<template>
  <div class="title-container" :class="{ 'animation-complete': phase === 'complete' }">
    <div 
      class="title"
      :style="{ 
        opacity: phase === 'initial' ? 0 : 1,
        transform: `scale(${phase === 'initial' ? 0.9 : 1})`
      }"
    >{{ text }}</div>
  </div>
</template>

<script>
const steps = [
  "TYSMP",
  "ThYSMP",
  "ThaYSMP",
  "ThanYSMP",
  "ThankYSMP",
  "ThankYSMP",
  "ThankYoSMP",
  "ThankYouSMP",
  "ThankYouSMP",
  "ThankYouSoMP",
  "ThankYouSoMP",
  "ThankYouSoMuP",
  "ThankYouSoMucP",
  "ThankYouSoMuchP",
  "ThankYouSoMuchP",
  "ThankYouSoMuchPe",
  "ThankYouSoMuchPer",
  "ThankYouSoMuchPers",
  "ThankYouSoMuchPerso",
  "ThankYouSoMuchPerson",
]

const reverseSteps = [
  "ThankYouSoMuchPerson",
  "ThankYouSoMuchPerso",
  "ThankYouSoMuchPers",
  "ThankYouSoMuchPer",
  "ThankYouSoMuchPe",
  "ThankYouSoMuchP",
  "ThankYouSoMucP",
  "ThankYouSoMuP",
  "ThankYouSoMP",
  "ThankYouSMP",
  "ThankYoSMP",
  "ThankYSMP",
  "ThanYSMP",
  "ThaYSMP",
  "ThYSMP",
  "TYSMP",
]

function getStepDelay(currentStep, totalSteps) {
  // Convert step to a position between 0 and 1
  const position = currentStep / (totalSteps - 1)
  
  // Base delay values
  const minDelay = 15 // Fastest speed (middle)
  const maxDelay = 45 // Slowest speed (ends)
  
  // Ease in-out curve
  // This creates a parabola that's slowest at the ends and fastest in the middle
  const easeInOut = 1 - 4 * Math.pow(position - 0.5, 2)
  
  // Calculate final delay
  return maxDelay - (maxDelay - minDelay) * easeInOut
}

export default {
  name: 'AnimatedTitle',
  data() {
    return {
      text: 'TYSMP',
      phase: 'initial',
      stepIndex: 0
    }
  },
  mounted() {
    // Fade in
    setTimeout(() => {
      this.phase = 'waiting'
    }, 1000)

    // Start animation after additional delay
    setTimeout(() => {
      this.phase = 'expanding'
      this.animate()
    }, 2000)
  },
  methods: {
    animate() {
      if (this.phase === 'expanding') {
        if (this.stepIndex < steps.length) {
          const delay = getStepDelay(this.stepIndex, steps.length)
          setTimeout(() => {
            this.text = steps[this.stepIndex]
            this.stepIndex++
            this.animate()
          }, delay)
        } else {
          this.phase = 'expanded'
          this.stepIndex = 0
          this.animate()
        }
      }
      
      else if (this.phase === 'expanded') {
        setTimeout(() => {
          this.phase = 'contracting'
          this.animate()
        }, 250)
      }
      
      else if (this.phase === 'contracting') {
        if (this.stepIndex < reverseSteps.length) {
          const delay = getStepDelay(this.stepIndex, reverseSteps.length)
          setTimeout(() => {
            this.text = reverseSteps[this.stepIndex]
            this.stepIndex++
            this.animate()
          }, delay)
        } else {
          this.phase = 'complete'
        }
      }
    }
  }
}
</script>

<style scoped>
.title-container {
  transform: translateY(0);
  transition: transform 0.5s ease-in-out;
}

.title-container.animation-complete {
  transform: translateY(-50px);
}

.title {
  color: white;
  font-size: clamp(3.375rem, 2.5vw, 6rem);
  font-weight: bold;
  white-space: nowrap;
  font-family: monospace;
  transition: opacity 0.3s ease-in-out, transform 0.3s ease-in-out;
}
</style> 