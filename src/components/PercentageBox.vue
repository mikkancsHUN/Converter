<template>
    <div class="percentage__box">
        <header class="title-header">
            <h2>Percentage % <span>Calculation</span></h2>
        </header>

        <p class="task-info">
            Calculate part from percentage and whole number:
        </p>
        <div class="percentage__box-item border-bot">
            <input v-model.number="percent" type="number" placeholder="(%)" />
            of
            <input v-model.number="whole" type="number" placeholder="(whole number)" />
            =
            <span class="output">{{ partResult ?? "" }}</span>
            <button @click="calculatePart">Calculate</button>
        </div>

        <p class="task-info">
            Calculate percent from part and whole number:
        </p>
        <div class="percentage__box-item border-bot">
            <input v-model.number="part" type="number" placeholder="(part)" />
            of
            <input v-model.number="whole2" type="number" placeholder="(whole number)" />
            =
            <span class="output">{{ percentResult ?? "" }}</span>
            <button @click="calculatePercent">Calculate</button>
        </div>

        <p class="task-info">
            Calculate whole number from part and percent:
        </p>
        <div class="percentage__box-item border-top">
            <input v-model.number="part2" type="number" placeholder="(part)" />
            as
            <input v-model.number="percent2" type="number" placeholder="(%)" />
            of =
            <span class="output">{{ wholeResult ?? "" }}</span>
            <button @click="calculateWhole">Calculate</button>
        </div>
        <button class="clear-btn" @click="clearAll">Clear</button>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
    name: 'PercentageBox',
    setup() {
        const percent = ref<number | null>(null)
        const whole = ref<number | null>(null)
        const partResult = ref<number | null>(null)

        const part = ref<number | null>(null)
        const whole2 = ref<number | null>(null)
        const percentResult = ref<number | null>(null)

        const part2 = ref<number | null>(null)
        const percent2 = ref<number | null>(null)
        const wholeResult = ref<number | null>(null)

        function calculatePart() {
            if (percent.value != null && whole.value != null) {
                partResult.value = (percent.value / 100) * whole.value
            } else {
                partResult.value = null
            }
        }

        function calculatePercent() {
            if (part.value != null && whole2.value != null && whole2.value !== 0) {
                percentResult.value = (part.value / whole2.value) * 100
            } else {
                percentResult.value = null
            }
        }

        function calculateWhole() {
            if (part2.value != null && percent2.value != null && percent2.value !== 0) {
                wholeResult.value = (part2.value / percent2.value) * 100
            } else {
                wholeResult.value = null
            }
        }

        function clearAll() {
            percent.value = null
            whole.value = null
            partResult.value = null
            part.value = null
            whole2.value = null
            percentResult.value = null
            part2.value = null
            percent2.value = null
            wholeResult.value = null
        }

        return {
            percent,
            whole,
            partResult,
            part,
            whole2,
            percentResult,
            part2,
            percent2,
            wholeResult,
            calculatePart,
            calculatePercent,
            calculateWhole,
            clearAll
        }
    }
})
</script>

<style scoped>
.percentage__box-item {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

/* Number input mezőkben lévő nyilak eltüntetése */
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

/* Firefox számára */
input[type="number"] {
    -moz-appearance: textfield;
}

.task-info {
    margin: 1rem 0 0 1rem;
    font-size: 0.9rem;
}

.task-info::before {
    content: '•';
    color: var(--clr-span);
    display: inline-block;
    width: 1em;
    margin-left: -1em;
}

.output {
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1em;
    margin: 1em 0.5em;
    height: 3.4em;
    width: 12em;
    background-color: var(--clr-inputfield);
    color: var(--clr-text);
    outline: none;
}

.clear-btn {
    margin: auto;
    margin-top: 1rem;
    display: block;
    width: 14rem;
    box-shadow: rgb(14, 10, 31) 0px 7.1166px 0px 0px;
}
</style>