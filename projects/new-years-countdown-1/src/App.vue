<script setup>
import { computed, onMounted, ref } from "vue";
import CountdownHeader from "@/components/CountdownHeader.vue";
import CountdownSegment from "@/components/CountdownSegment.vue";

const timeLeftInSeconds = ref(0)

onMounted(()=> {
  const nextYear = new Date().getFullYear() + 1
  const newYearsDate = new Date(`${nextYear}-01-01`)
  newYearsDate.setHours(0, 0, 0, 0)
  setInterval(() => {
    const nowDate = new Date()
    const dateDiff = Math.floor((newYearsDate.getTime() - nowDate.getTime()) / 1000)
    timeLeftInSeconds.value = dateDiff
  }, 1000)
})

const formatTime = (seconds, format) => {
  const daysLeft = seconds / 60 / 60 / 24
  const hoursLeft = (daysLeft - Math.floor(daysLeft)) * 24
  const minutesLeft = (hoursLeft - Math.floor(hoursLeft)) * 60
  const secondsLeft = Math.floor(seconds % 60)

  if (format === 'DD') {
    return Math.floor(daysLeft)
  } else if (format === 'HH') {
    return Math.floor(hoursLeft)
  } else if (format === 'MM') {
    return Math.floor(minutesLeft)
  } else {
    return secondsLeft
  }
}

</script>
<template>
  <div class="app-wrapper">
    <div class="countdown-box">
      <CountdownHeader />

      <main class="flex justify-center">
        <CountdownSegment data-test="days" label="days" :number="formatTime(timeLeftInSeconds, 'DD')" />
        <CountdownSegment data-test="hours" label="hours" :number="formatTime(timeLeftInSeconds, 'HH')" />
        <CountdownSegment data-test="minutes" label="minutes" :number="formatTime(timeLeftInSeconds, 'MM')" />
        <CountdownSegment data-test="seconds" label="seconds" :number="formatTime(timeLeftInSeconds, 'SS')" />
      </main>
    </div>
  </div>
</template>

<style scoped>
.app-wrapper {
  @apply flex items-center justify-center w-full h-full p-10;
}
.countdown-box {
  @apply shadow-md relative bg-white p-5 rounded-lg border-gray-100 border-[1px];
}
body {
  @apply bg-gray-100;
}
</style>
