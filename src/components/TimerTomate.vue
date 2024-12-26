<template>
    <div class="row justify-center">
        <q-circular-progress
            show-value
            font-size="80px"
            :value="progressValue"
            size="320px"
            :thickness="0.1"
            color="light-blue"
            track-color="transparent"
            class="q-ma-md text-green-13 text-bold"
        >
            {{ displayTime }}
        </q-circular-progress>
    </div>
    <div class="row justify-center">
        <q-btn class="q-ma-md" color="green" label="Start" @click="startTimer" />
        <q-btn class="q-ma-md" color="grey" label="Pause" @click="pauseTimer" />
        <q-btn class="q-ma-md" color="red" label="Reset" @click="resetTimer" />
    </div>
    <div class="row justify-center">
        <q-btn class="q-ma-md" color="blue" label="Short Break (5min)" @click="setTimer(5)" />
        <q-btn class="q-ma-md" color="purple" label="Long Break (15min)" @click="setTimer(15)" />
    </div>
</template>
<script setup>
import { ref, onUnmounted, watch, computed } from 'vue'
const props = defineProps({
    minutes: {
        type: Number,
        required: true,
    },
    smallIntval: {
        type: Number,
        default: 5000,
    },
    bigIntval: {
        type: Number,
        default: 30000,
    },
})

const minutes = ref(props.minutes)
const seconds = ref(0)
const countdown = ref(props.minutes * 60)
let timer = null

// sound feedbacks
// const blip = new Audio(require('@/assets/sounds/blip.mp3'))
const tick = new Audio('../sounds/tick.mp3')
const buzz = new Audio('..//sounds/buzz.mp3')

const progressValue = computed(() => {
    return (countdown.value / (props.minutes * 60)) * 100
})

const displayTime = computed(() => {
    return `${minutes.value.toString().padStart(2, '0')}:${seconds.value.toString().padStart(2, '0')}`
})

const clearInterval = () => {
    if (timer) {
        window.clearInterval(timer)
        timer = null
    }
}

const pauseTimer = () => {
    clearInterval()
}

const setTimer = (mins) => {
    clearInterval()
    countdown.value = mins * 60
    minutes.value = mins
    seconds.value = 0
    displayTime()
}

const resetTimer = () => {
    clearInterval()
    countdown.value = props.minutes * 60
    minutes.value = props.minutes
    seconds.value = 0
}

const startTimer = () => {
    clearInterval()
    tick.play()
    timer = window.setInterval(() => {
        if (countdown.value > 0) {
            countdown.value--
            if (seconds.value === 0) {
                minutes.value--
                seconds.value = 59
            } else {
                seconds.value--
            }
        } else {
            clearInterval()
            buzz.play()
        }
    }, 1000)
}

watch(
    () => props.minutes,
    (newValue) => {
        minutes.value = newValue
        seconds.value = 0
        countdown.value = newValue * 60
        clearInterval()
    },
)

onUnmounted(() => {
    clearInterval()
})
</script>
