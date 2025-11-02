<script setup lang="ts">
import { ref, computed } from 'vue'
import actuallyMeme from './assets/actuallyMEME.png'
import retardMeme from './assets/retardMEME.png'

const inputText = ref('')

// Convert text to retard speak: every other letter capitalized (except first and last of each word)
const convertedText = computed(() => {
  if (!inputText.value) return ''
  
  // Split by word boundaries (spaces, punctuation, etc.)
  return inputText.value
    .split(/(\s+|[^\w\s])/)
    .map(segment => {
      // If it's whitespace or punctuation, keep as is
      if (!/\w/.test(segment)) {
        return segment
      }
      
      // Process words only
      const chars = segment.split('')
      if (chars.length <= 2) {
        // Words with 2 or fewer letters stay unchanged
        return segment
      }
      
      // Process each letter: first and last stay lowercase, middle letters alternate
      return chars
        .map((char, index) => {
          // First and last letters stay as is (but force lowercase)
          if (index === 0 || index === chars.length - 1) {
            return char.toLowerCase()
          }
          
          // Middle letters: capitalize every other one (starting with index 1)
          // Index 1, 3, 5, etc. get capitalized (odd indices in the middle)
          const isOddIndex = (index - 1) % 2 === 0
          return isOddIndex ? char.toUpperCase() : char.toLowerCase()
        })
        .join('')
    })
    .join('')
})
</script>

<template>
  <div class="container">
    <h1 class="title">Retard Speak Converter</h1>
    
    <div class="converter-box">
      <div class="input-section">
        <label for="input-text" class="label">Input Text</label>
        <textarea
          id="input-text"
          v-model="inputText"
          class="text-box"
          placeholder="Type your text here..."
          rows="20"
        ></textarea>
      </div>
      
      <div class="output-section">
        <label for="output-text" class="label">Output Text</label>
        <textarea
          id="output-text"
          :value="convertedText"
          class="text-box output"
          placeholder="Converted text will appear here..."
          rows="20"
          readonly
        ></textarea>
      </div>
    </div>
    
    <div class="meme-container">
      <img :src="actuallyMeme" alt="actually meme" class="meme-image actually-meme" />
      <img :src="retardMeme" alt="retard drawing meme" class="meme-image" />
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: calc(100vh - 4rem);
}

.title {
  text-align: center;
  margin-bottom: 2rem;
  font-size: 2rem;
  color: hsla(160, 100%, 37%, 1);
  transition: color 0.3s;
}

.converter-box {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  width: 100%;
  margin-bottom: 2rem;
}

.meme-container {
  display: flex;
  gap: 2rem;
  width: 100%;
  justify-content: center;
  align-items: center;
  margin-top: 2rem;
  flex-wrap: wrap;
}

.meme-image {
  width: 300px;
  height: 300px;
  object-fit: contain;
  border-radius: 8px;
  background: transparent;
  border: none;
  outline: none;
  flex: 1;
  max-width: 100%;
}

.input-section,
.output-section {
  display: flex;
  flex-direction: column;
}

.label {
  font-size: 1.1rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: var(--color-text);
  transition: color 0.3s;
}

.text-box {
  width: 100%;
  padding: 1rem;
  font-size: 1rem;
  font-family: inherit;
  border: 2px solid var(--color-border);
  border-radius: 8px;
  resize: vertical;
  transition: border-color 0.3s, background-color 0.3s, color 0.3s;
  background-color: var(--color-background);
  color: var(--color-text);
}

.text-box:focus {
  outline: none;
  border-color: hsla(160, 100%, 37%, 1);
}

.text-box.output {
  background-color: var(--color-background-soft);
  cursor: default;
}

.text-box::placeholder {
  color: var(--color-text);
  opacity: 0.5;
}

/* Dark mode specific adjustments */
@media (prefers-color-scheme: dark) {
  .text-box {
    border-color: var(--color-border);
  }
  
  .text-box.output {
    background-color: var(--color-background-mute);
  }
}

/* Responsive design */
@media (max-width: 768px) {
  .converter-box {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .container {
    padding: 1rem;
  }

  .title {
    font-size: 1.5rem;
    margin-bottom: 1.5rem;
  }

  .meme-container {
    flex-direction: column;
    gap: 1.5rem;
  }
  
  .meme-image {
    width: 100%;
    max-width: 250px;
    height: 250px;
  }
}
</style>
