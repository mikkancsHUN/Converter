<template>
  <div class="VueApp">
    <header class="app-header">
      <h1>Smart Calculator <span class="highlight">Pro</span></h1>
      <p class="app-subtitle">Percentage & Basic Calculator</p>
    </header>

    <main class="main-content">
      <PercentageBox class="card" />
      <CalculatorBox class="card" />
    </main>

    <footer class="app-footer">
      <p>Made with ❤️ using Vue.js</p>
      <p class="version">v1.0.0</p>
    </footer>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import PercentageBox from './components/PercentageBox.vue'
import CalculatorBox from './components/CalculatorBox.vue'

export default defineComponent({
  name: 'App',
  components: {
    PercentageBox,
    CalculatorBox
  }
})
</script>

<style scoped>
.VueApp {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: var(--clr-background);
  color: var(--clr-text);
}

.app-header {
  text-align: center;
  padding: 2rem 1rem;
  background: linear-gradient(to right,
      rgba(26, 26, 46, 0.8),
      rgba(22, 33, 62, 0.8));
  backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.app-subtitle {
  color: var(--clr-text-secondary);
  font-size: 1.1rem;
  margin-top: 0.5rem;
}

.main-content {
  flex: 1;
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
  align-items: flex-start;
  justify-content: center;
  padding: 2rem;
  max-width: 1400px;
  margin: 0 auto;
  width: 100%;
}

.app-footer {
  text-align: center;
  padding: 1.5rem;
  color: var(--clr-text-secondary);
  font-size: 0.9rem;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  background: rgba(0, 0, 0, 0.2);
}

.version {
  opacity: 0.7;
  font-size: 0.8rem;
  margin-top: 0.5rem;
}

@media screen and (max-width: 768px) {
  .main-content {
    flex-direction: column;
    align-items: center;
    padding: 1rem;
    gap: 1.5rem;
  }

  .app-header {
    padding: 1.5rem 1rem;
  }

  h1 {
    font-size: 1.8rem;
  }
}
</style>