<template>
    <div class="calculator__box">
        <h2>Calculator <span>/ - + *</span></h2>
        <input v-model="input" type="text" id="inputwindow" />
        <div class="keyboard">
            <button v-for="btn in buttons" class="keyboard__button" :key="btn" @click="append(btn)">
                {{ btn }}
            </button>
            <button class="clear-btn keyboard__button" @click="clearInput">Clear</button>
            <button class="equal-btn keyboard__button" @click="calculateResult">=</button>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
    name: 'CalculatorBox',
    setup() {
        const input = ref('')

        const buttons = ['1', '2', '3', '+', '4', '5', '6', '-', '7', '8', '9', '*', '0', '.', '/', '%']

        function append(value: string) {
            input.value += value
        }

        function clearInput() {
            input.value = ''
        }

        function calculateResult() {
            try {
                // Figyelem: eval() használata nem ajánlott éles környezetben
                const result = eval(input.value)
                input.value = result !== undefined ? String(result) : ''
            } catch {
                input.value = 'Error'
            }
        }

        return {
            input,
            buttons,
            append,
            clearInput,
            calculateResult
        }
    }
})
</script>

<style scoped>
.calculator__box {
    background-color: rgba(0, 0, 0, 0.3);
    padding: 2em;
    border-radius: 20px;
    min-width: 300px;
}

.keyboard {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(5, 1fr);
    gap: 0;
}

.keyboard__button {
    margin: 0 0 12px 0;
}

.equal-btn {
    grid-column: span 1;
}

.clear-btn {
    grid-column: span 3;
}

#inputwindow {
    width: 100%;
    height: 100px;
    border-radius: 10px;
    font-size: 2em;
    margin: 1.2em 0;
}
</style>