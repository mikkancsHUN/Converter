<template>
    <div class="calculator__box">
        <h2>Calculator <span>/ - + *</span></h2>
        <div class="input-container">
            <div class="equation-display" v-if="equation">
                {{ equation }}
            </div>
            <input v-model="display" type="text" class="inputwindow" readonly />
        </div>
        <div class="keyboard">
            <button v-for="btn in buttons" class="keyboard__button" :key="btn" @click="handleButton(btn)">
                {{ btn }}
            </button>
            <button class="clear-btn keyboard__button" @click="clearAll">Clear</button>
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
        // Jelenlegi kijelző érték
        const display = ref('0')
        // Teljes egyenlet a kijelző felett
        const equation = ref('')

        // Belső állapotok
        const firstOperand = ref<number | null>(null)
        const operator = ref<string | null>(null)
        const waitingForSecondOperand = ref(false)
        const shouldResetDisplay = ref(false)

        // Gombok ugyanazok
        const buttons = ['1', '2', '3', '+', '4', '5', '6', '-', '7', '8', '9', '*', '0', '.', '/', '%']

        // Szám gomb kezelése
        function inputDigit(digit: string) {
            if (shouldResetDisplay.value) {
                display.value = digit
                shouldResetDisplay.value = false
            } else {
                display.value = display.value === '0' ? digit : display.value + digit
            }
            waitingForSecondOperand.value = false
        }

        // Tizedesvessző kezelése
        function inputDecimal() {
            if (shouldResetDisplay.value) {
                display.value = '0.'
                shouldResetDisplay.value = false
                return
            }

            if (!display.value.includes('.')) {
                display.value += '.'
            }
        }

        // Operátor kezelése
        function handleOperator(nextOperator: string) {
            const inputValue = parseFloat(display.value)

            if (operator.value && waitingForSecondOperand.value) {
                operator.value = nextOperator
                equation.value = `${firstOperand.value} ${operator.value}`
                return
            }

            if (firstOperand.value === null) {
                firstOperand.value = inputValue
            } else if (operator.value) {
                const result = performCalculation()
                display.value = `${result}`
                firstOperand.value = result
            }

            waitingForSecondOperand.value = true
            operator.value = nextOperator
            equation.value = `${firstOperand.value} ${operator.value}`
            shouldResetDisplay.value = true
        }

        // Számítás végrehajtása
        function performCalculation(): number {
            const inputValue = parseFloat(display.value)

            if (firstOperand.value === null || operator.value === null) {
                return inputValue
            }

            switch (operator.value) {
                case '+':
                    return firstOperand.value + inputValue
                case '-':
                    return firstOperand.value - inputValue
                case '*':
                    return firstOperand.value * inputValue
                case '/':
                    if (inputValue === 0) {
                        throw new Error('Division by zero')
                    }
                    return firstOperand.value / inputValue
                case '%':
                    return firstOperand.value % inputValue
                default:
                    return inputValue
            }
        }

        // Százalék kezelése
        function handlePercentage() {
            const currentValue = parseFloat(display.value)
            display.value = (currentValue / 100).toString()
        }

        // Fő gombkezelő függvény
        function handleButton(value: string) {
            // Számok
            if (/^[0-9]$/.test(value)) {
                inputDigit(value)
                return
            }

            // Tizedesvessző
            if (value === '.') {
                inputDecimal()
                return
            }

            // Operátorok
            if (['+', '-', '*', '/'].includes(value)) {
                handleOperator(value)
                return
            }

            // Százalék
            if (value === '%') {
                handlePercentage()
                return
            }
        }

        // Egyenlőségjel
        function calculateResult() {
            if (operator.value === null || firstOperand.value === null) {
                return
            }

            try {
                const result = performCalculation()
                equation.value = `${firstOperand.value} ${operator.value} ${display.value} =`
                display.value = formatResult(result)

                // Reset állapotok
                firstOperand.value = null
                operator.value = null
                waitingForSecondOperand.value = false
                shouldResetDisplay.value = true
            } catch (error) {
                display.value = 'Error'
                resetCalculator()
            }
        }

        // Eredmény formázása - optimális változat
        function formatResult(result: number): string {
            // Túl nagy vagy túl kis számok kezelése
            if (Math.abs(result) > 1e12 || (Math.abs(result) < 1e-6 && result !== 0)) {
                return result.toExponential(6)
            }

            // Tizedesjegyek számának korlátozása (max 8 tizedesjegy)
            const fixedResult = result.toFixed(8)
            // Levágjuk a felesleges nullákat a végéről
            return fixedResult.replace(/(\.\d*?)0+$/, '$1').replace(/\.$/, '')
        }

        // Visszalépés
        function backspace() {
            if (display.value === 'Error' || display.value.length === 1) {
                display.value = '0'
            } else {
                display.value = display.value.slice(0, -1)
            }
        }

        // Minden törlése
        function clearAll() {
            display.value = '0'
            equation.value = ''
            firstOperand.value = null
            operator.value = null
            waitingForSecondOperand.value = false
            shouldResetDisplay.value = false
        }

        // Hiba esetén reset
        function resetCalculator() {
            setTimeout(() => {
                clearAll()
            }, 1500)
        }

        return {
            display,
            equation,
            buttons,
            handleButton,
            clearAll,
            backspace,
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
    width: 90%;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

.inputwindow {
    width: 100%;
    height: 100px;
    border-radius: 10px;
    font-size: 2em;
    margin: 1.2em 0;
    box-sizing: border-box;
    padding-top: 30px;
    text-align: right;
    padding-right: 15px;
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