<template>
    <div class="calculator__box">
        <h2>Calculator <span>/ - + *</span></h2>
        <div class="input-container">
            <div class="equation-display" v-if="equation">
                {{ equation }}
            </div>
            <input v-model="input" type="text" class="inputwindow" />
        </div>
        <div class="keyboard">
            <button v-for="btn in buttons" class="keyboard__button" :key="btn" @click="append(btn)">
                {{ btn }}
            </button>
            <button class="clear-btn keyboard__button" @click="clearInput">Clear</button>
            <button class="cancel-btn keyboard__button" @click="backspace">⌫</button>
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
        const equation = ref('')
        const isResultDisplayed = ref(false)
        const lastExpression = ref('') // Új: az utolsó kifejezés mentése

        const buttons = ['1', '2', '3', '+', '4', '5', '6', '-', '7', '8', '9', '*', '0', '.', '/', '%']

        function append(value: string) {
            if (isResultDisplayed.value) {
                input.value = ''
                equation.value = ''
                isResultDisplayed.value = false
            }
            input.value += value
        }

        function handleCancel() {
            if (isResultDisplayed.value) {
                // Ha eredmény van kijelenítve, visszatérünk az eredeti kifejezéshez
                input.value = lastExpression.value
                isResultDisplayed.value = false
                equation.value = ''
            } else {
                // Egyébként sima backspace
                if (input.value.length > 0) {
                    input.value = input.value.slice(0, -1)
                }
            }
        }

        function clearInput() {
            input.value = ''
            equation.value = ''
            lastExpression.value = ''
            isResultDisplayed.value = false
        }

        function calculateResult() {
            if (!input.value.trim()) return

            try {
                // Elmentjük az eredeti kifejezést
                lastExpression.value = input.value
                equation.value = input.value
                const result = eval(input.value)
                input.value = result !== undefined ? String(result) : ''
                isResultDisplayed.value = true
            } catch {
                input.value = 'Error'
                equation.value = ''
                lastExpression.value = ''
                isResultDisplayed.value = false
            }
        }

        return {
            input,
            equation,
            buttons,
            append,
            handleCancel,
            clearInput,
            backspace: handleCancel,
            calculateResult
        }
    }
})
</script>

<style scoped>
.input-container {
    position: relative;
    width: 100%;
}

.equation-display {
    position: absolute;
    top: 50px;
    left: 15px;
    font-size: 0.9em;
    color: rgba(255, 255, 255, 0.6);
    pointer-events: none;
    z-index: 2;
    padding: 2px 8px;
    border-radius: 3px;
}

.inputwindow {
    width: 100%;
    height: 100px;
    border-radius: 10px;
    font-size: 2em;
    margin: 1.2em 0;
    box-sizing: border-box;
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

.clear-btn {
    grid-column: span 2;
}
</style>