<template>
  <button 
    class="glass-button"
    :class="{ 'visible': isVisible }"
    @mouseover="isHovered = true"
    @mouseleave="isHovered = false"
    @mousedown="isPressed = true"
    @mouseup="isPressed = false"
    :style="{
      transform: `scale(${isPressed ? 0.98 : isHovered ? 1.02 : 1})`
    }"
  >
    <div class="glass-edge"></div>
    <div class="button-content">
      <span class="sparkle">✧</span>
      <span class="button-text">APPLY</span>
    </div>
  </button>
</template>

<script>
export default {
  name: 'GlassButton',
  props: {
    visible: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      isHovered: false,
      isPressed: false,
      isVisible: false
    }
  },
  mounted() {
    // Add a small delay before showing the button for a nice fade-in effect
    setTimeout(() => {
      this.isVisible = this.visible
    }, 100)
  },
  watch: {
    visible(newValue) {
      this.isVisible = newValue
    }
  }
}
</script>

<style scoped>
.glass-button {
  min-width: 600px;
  padding: 3rem 10rem;
  border-radius: 100px;
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
  
  /* Enhanced glass effect */
  box-shadow: 
    0 2px 4px rgba(0, 0, 0, 0.1),
    0 -1px 4px rgba(255, 255, 255, 0.15),
    inset 0 2px 4px rgba(255, 255, 255, 0.25),
    inset 0 -2px 4px rgba(0, 0, 0, 0.15);
  
  /* Initial state */
  opacity: 0;
  transform: translateY(20px);
}

/* Enhanced refractive edge */
.glass-edge {
  position: absolute;
  top: -2px;
  left: -2px;
  right: -2px;
  bottom: -2px;
  border-radius: inherit;
  pointer-events: none;
  background: linear-gradient(
    135deg,
    rgba(255, 255, 255, 0.4) 0%,
    rgba(255, 255, 255, 0.1) 50%,
    rgba(255, 255, 255, 0) 100%
  );
  opacity: 0.5;
  filter: blur(4px);
}

.glass-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.25) 0%,
    rgba(255, 255, 255, 0.02) 100%
  );
  border-radius: inherit;
}

.glass-button::after {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  right: -50%;
  bottom: -50%;
  background: radial-gradient(
    circle at center,
    rgba(255, 255, 255, 0.2) 0%,
    rgba(255, 255, 255, 0) 70%
  );
  opacity: 0;
  transition: opacity 0.3s ease;
}

.glass-button.visible {
  opacity: 1;
  transform: translateY(0);
}

.button-content {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  mix-blend-mode: overlay;
}

.sparkle {
  font-size: 2rem;
  opacity: 0.9;
  color: rgba(255, 255, 255, 1);
  text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
}

.button-text {
  font-size: 3rem;
  font-weight: 500;
  letter-spacing: 0.2em;
  color: rgba(255, 255, 255, 1);
  text-shadow: 
    0 0 20px rgba(255, 255, 255, 0.3),
    0 1px 2px rgba(0, 0, 0, 0.1);
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

.glass-button:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(255, 255, 255, 0.4);
  box-shadow: 
    0 4px 8px rgba(0, 0, 0, 0.2),
    0 -2px 6px rgba(255, 255, 255, 0.2),
    inset 0 2px 4px rgba(255, 255, 255, 0.3),
    inset 0 -2px 4px rgba(0, 0, 0, 0.2);
}

.glass-button:hover::after {
  opacity: 1;
}

.glass-button:active {
  background: rgba(255, 255, 255, 0.03);
  border-color: rgba(255, 255, 255, 0.2);
  box-shadow: 
    0 1px 2px rgba(0, 0, 0, 0.1),
    inset 0 1px 2px rgba(255, 255, 255, 0.15),
    inset 0 -1px 2px rgba(0, 0, 0, 0.1);
}

/* Disable hover effects on mobile */
@media (hover: none) {
  .glass-button:hover {
    transform: none;
    background: rgba(255, 255, 255, 0.05);
    border-color: rgba(255, 255, 255, 0.3);
    box-shadow: 
      0 2px 4px rgba(0, 0, 0, 0.1),
      0 -1px 4px rgba(255, 255, 255, 0.15),
      inset 0 2px 4px rgba(255, 255, 255, 0.25),
      inset 0 -2px 4px rgba(0, 0, 0, 0.15);
  }
}
</style> 