<template>
  <div class="VueApp">
    <main class="main-content">
      <PercentageBox class="card" />
      <CalculatorBox class="card" />
    </main>
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

.main-content {
  flex: 1;
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  margin: 0 auto;
  width: 100%;
}

@media screen and (max-width: 768px) {
  .main-content {
    flex-direction: column;
    align-items: center;
    padding: 1rem;
    gap: 1.5rem;
  }

  h1 {
    font-size: 1.8rem;
  }
}
</style>