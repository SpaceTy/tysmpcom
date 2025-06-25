<template>
  <div class="form-container" :class="{ 'visible': isVisible }">
    <form @submit.prevent="handleSubmit" class="glass-form">
      <div class="form-group">
        <label for="discordUsername">Discord Username</label>
        <input 
          type="text" 
          id="discordUsername" 
          v-model="formData.discordUsername"
          placeholder="username#0000"
          required
          pattern=".+#[0-9]{4}"
        >
      </div>

      <div class="form-group">
        <label for="minecraftUsername">Minecraft Username</label>
        <input 
          type="text" 
          id="minecraftUsername" 
          v-model="formData.minecraftUsername"
          required
        >
      </div>

      <div class="form-group">
        <label for="age">Age</label>
        <input 
          type="number" 
          id="age" 
          v-model.number="formData.age"
          min="13"
          required
        >
      </div>

      <div class="form-group">
        <label for="favouriteAboutMinecraft">What's your favourite thing about Minecraft?</label>
        <textarea 
          id="favouriteAboutMinecraft" 
          v-model="formData.favouriteAboutMinecraft"
          required
        ></textarea>
      </div>

      <div class="form-group">
        <label for="understandingOfConcept">How well do you understand our server's concept?</label>
        <select 
          id="understandingOfConcept" 
          v-model="formData.understandingOfConcept"
          required
        >
          <option value="">Please select...</option>
          <option value="very-well">Very Well</option>
          <option value="well">Well</option>
          <option value="somewhat">Somewhat</option>
          <option value="need-more-info">Need More Information</option>
        </select>
      </div>

      <div class="form-group">
        <label for="favouriteRule">Which server rule resonates with you the most?</label>
        <textarea 
          id="favouriteRule" 
          v-model="formData.favouriteRule"
          required
        ></textarea>
      </div>

      <div class="error-message" v-if="errorMessage">
        ❌ {{ errorMessage }}
      </div>

      <div class="success-message" v-if="successMessage">
        ✨ {{ successMessage }}
      </div>

      <button type="submit" class="submit-button" :disabled="isSubmitting">
        {{ isSubmitting ? 'Submitting...' : 'Submit Application' }}
      </button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'ApplicationForm',
  props: {
    visible: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      isVisible: false,
      isSubmitting: false,
      errorMessage: '',
      successMessage: '',
      formData: {
        discordUsername: '',
        minecraftUsername: '',
        age: null,
        favouriteAboutMinecraft: '',
        understandingOfConcept: '',
        favouriteRule: ''
      }
    }
  },
  mounted() {
    // Load any existing data from localStorage
    this.loadStoredData()
    
    setTimeout(() => {
      this.isVisible = this.visible
    }, 100)
  },
  watch: {
    visible(newValue) {
      this.isVisible = newValue
    },
    // Auto-save form data as user types
    'formData': {
      handler(newData) {
        this.saveToLocalStorage(newData)
      },
      deep: true
    }
  },
  methods: {
    loadStoredData() {
      try {
        const storedData = localStorage.getItem('applicationFormData')
        if (storedData) {
          this.formData = JSON.parse(storedData)
        }
      } catch (error) {
        console.error('❌ Error loading stored data:', error)
      }
    },
    saveToLocalStorage(data) {
      try {
        localStorage.setItem('applicationFormData', JSON.stringify(data))
      } catch (error) {
        console.error('❌ Error saving data:', error)
      }
    },
    validateForm() {
      if (!this.formData.discordUsername.includes('#')) {
        this.errorMessage = 'Please enter a valid Discord username (including #0000)'
        return false
      }
      if (this.formData.age < 13) {
        this.errorMessage = 'You must be at least 13 years old'
        return false
      }
      return true
    },
    async handleSubmit() {
      this.errorMessage = ''
      this.successMessage = ''
      
      if (!this.validateForm()) {
        return
      }

      this.isSubmitting = true

      try {
        // Here you would typically send the data to your backend
        // For now, we'll just store it in localStorage
        await this.simulateApiCall()
        
        this.successMessage = 'Application submitted successfully! ✨'
        localStorage.removeItem('applicationFormData') // Clear stored data after successful submission
        
        // Reset form after successful submission
        setTimeout(() => {
          Object.keys(this.formData).forEach(key => {
            this.formData[key] = typeof this.formData[key] === 'number' ? null : ''
          })
        }, 2000)
      } catch (error) {
        this.errorMessage = 'Failed to submit application. Please try again.'
        console.error('❌ Submission error:', error)
      } finally {
        this.isSubmitting = false
      }
    },
    simulateApiCall() {
      // Simulate an API call with a promise
      return new Promise((resolve) => {
        setTimeout(resolve, 1000)
      })
    }
  }
}
</script>

<style scoped>
.form-container {
  max-width: 800px;
  width: 90%;
  margin: 0 auto;
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.3s ease;
}

.form-container.visible {
  opacity: 1;
  transform: translateY(0);
}

.glass-form {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 20px;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

label {
  color: rgba(255, 255, 255, 0.9);
  font-size: 1rem;
  font-weight: 500;
}

input, textarea, select {
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 10px;
  padding: 0.75rem 1rem;
  color: white;
  font-size: 1rem;
  transition: all 0.2s ease;
}

textarea {
  min-height: 100px;
  resize: vertical;
}

input:focus, textarea:focus, select:focus {
  outline: none;
  border-color: rgba(255, 255, 255, 0.4);
  background: rgba(255, 255, 255, 0.15);
}

.error-message {
  color: #ff6b6b;
  font-size: 0.9rem;
  padding: 0.5rem;
  background: rgba(255, 107, 107, 0.1);
  border-radius: 8px;
}

.success-message {
  color: #51cf66;
  font-size: 0.9rem;
  padding: 0.5rem;
  background: rgba(81, 207, 102, 0.1);
  border-radius: 8px;
}

.submit-button {
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 10px;
  padding: 1rem;
  color: white;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.2s ease;
}

.submit-button:hover:not(:disabled) {
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.3);
}

.submit-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* Style the dropdown options */
select option {
  background: #1a1a1a;
  color: white;
}

/* Responsive adjustments */
@media (max-width: 600px) {
  .glass-form {
    padding: 1.5rem;
  }
  
  input, textarea, select, .submit-button {
    padding: 0.6rem 0.8rem;
  }
}
</style> 