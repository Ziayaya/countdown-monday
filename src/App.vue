<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const timeLeft = ref({ days: 0, hours: 0, minutes: 0, seconds: 0 })
let timer = null

const calculateTimeLeft = () => {
  const now = new Date()
  const nextMonday = new Date()
  
  // Mencari hari Senin mendatang (Senin = 1)
  // (8 - hari_ini) % 7 memberikan sisa hari ke Senin depan
  const dayOffset = (8 - now.getDay()) % 7
  const targetDate = dayOffset === 0 ? 7 : dayOffset // Jika hari ini senin, target senin depan
  
  nextMonday.setDate(now.getDate() + targetDate)
  nextMonday.setHours(0, 0, 0, 0)

  const difference = nextMonday - now

  if (difference > 0) {
    timeLeft.value = {
      days: Math.floor(difference / (1000 * 60 * 60 * 24)),
      hours: Math.floor((difference / (1000 * 60 * 60)) % 24),
      minutes: Math.floor((difference / 1000 / 60) % 60),
      seconds: Math.floor((difference / 1000) % 60)
    }
  }
}

onMounted(() => {
  calculateTimeLeft()
  timer = setInterval(calculateTimeLeft, 1000)
})

onUnmounted(() => {
  clearInterval(timer)
})
</script>

<template>
  <div class="container">
    <h1>Menuju Hari Senin</h1>
    <div class="timer">
      <div class="box"><span>{{ timeLeft.days }}</span> Hari</div>
      <div class="box"><span>{{ timeLeft.hours }}</span> Jam</div>
      <div class="box"><span>{{ timeLeft.minutes }}</span> Menit</div>
      <div class="box"><span>{{ timeLeft.seconds }}</span> Detik</div>
    </div>
  </div>
</template>

<style scoped>
.container { text-align: center; font-family: sans-serif; padding-top: 50px; }
.timer { display: flex; justify-content: center; gap: 15px; margin-top: 20px; }
.box { background: #42b883; color: white; padding: 20px; border-radius: 8px; min-width: 80px; }
.box span { display: block; font-size: 2rem; font-weight: bold; }
</style>