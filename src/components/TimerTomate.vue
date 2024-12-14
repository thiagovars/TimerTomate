<template>
  <div class="container">
    <div class="header">
      <span class="header">Start Pomodoro</span>
    </div>
    <div class="timer-container">
      <div class="display-timer">{{ displayTime }}</div>
    </div>
    <div class="timer-controlls container">
      <button class="start-button button green" @click="startTimer">Start</button>
      <button class="stop-button button red" @click="stopTimer">Stop</button>
      <button class="reset-button button" @click="resetTimer">Reset</button>
    </div>
  </div>
</template>

<script lang="js">
export default {
  name: 'TimerTomate',
  data() {
    return {
      time: 25 * 60, // 25 minutes in seconds
      timer: null,
      isRunning: false,
    }
  },
  computed: {
    displayTime() {
      const hours = Math.floor(this.time / 3600)
      const minutes = Math.floor((this.time % 3600) / 60)
      const seconds = this.time % 60
      return `${hours.toString().padStart(2, '0')}:${minutes
        .toString()
        .padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
    },
  },
  methods: {
    startTimer() {
      if (!this.isRunning) {
        this.isRunning = true
        this.timer = setInterval(() => {
          if (this.time > 0) {
            this.time--
          } else {
            this.stopTimer()
          }
        }, 1000)
      }
    },
    stopTimer() {
      this.isRunning = false
      clearInterval(this.timer)
    },
    resetTimer() {
      this.stopTimer()
      this.time = 25 * 60
    },
  },
  beforeDestroy() {
    this.stopTimer()
  },
}
</script>
<style scoped>
.header {
  font-size: 4.5rem;
  font-weight: bolder;
  margin-bottom: 1rem;
}

.display-timer {
  font-size: 5rem;
  font-weight: bolder;
  margin-bottom: 1rem;
  color: #fff;
}

.timer-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 1rem;
}

.timer-controlls {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 1rem;
}

.button {
  font-size: 1.5rem;
  font-weight: bold;
  padding: 0.5rem 1rem;
  margin: 0 0.5rem;
  border: none;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: background-color 0.2s ease-in-out;
}

.red {
  background-color: rgb(172, 92, 92);
}
.green {
  background-color: rgb(92, 172, 92);
}
</style>
