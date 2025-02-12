<script setup>
import { ref, computed, onMounted } from "vue";

// Function to get current time in the client's timezone
function getCurrentTime() {
  const now = new Date();
  return now;
}

// Function to calculate the countdown to a target date (Valentine's Day, for example)
function getCountdownTimer(targetDate) {
  const now = new Date(); // Get current time
  const target = new Date(targetDate); // Convert the target date string to a Date object
  const timeLeft = target - now; // Calculate difference between target and current time

  if (timeLeft < 0) {
    return { days: 0, hours: 0, minutes: 0, seconds: 0 }; // No time left
  }

  // Calculate days, hours, minutes, seconds
  const days = Math.floor(timeLeft / (1000 * 60 * 60 * 24));
  const hours = Math.floor((timeLeft / (1000 * 60 * 60)) % 24);
  const minutes = Math.floor((timeLeft / 1000 / 60) % 60);
  const seconds = Math.floor((timeLeft / 1000) % 60);

  return { days, hours, minutes, seconds };
}

// Reactive state to store current time and countdown time
const currentTime = ref(getCurrentTime());
const countdownTargetDate = "2025-02-14T00:00:00"; // Set your target date for countdown (e.g., Valentine's Day)
const countdownTime = ref(getCountdownTimer(countdownTargetDate));

// Update the current time every second
onMounted(() => {
  setInterval(() => {
    currentTime.value = getCurrentTime(); // Refresh the current time
    countdownTime.value = getCountdownTimer(countdownTargetDate); // Refresh the countdown
  }, 1000);
});
</script>

<template>
  <div class="countdown-container">
    <h2>До Дня Святого Валентина</h2>
    <div class="countdown">
      <span>{{ countdownTime.days }} дн </span> :
      <span>{{ countdownTime.hours }} ч </span> :
      <span>{{ countdownTime.minutes }} мин </span> :
      <span>{{ countdownTime.seconds }} с </span>
    </div>
  </div>
</template>

<style scoped>
.countdown-container {
  font-family: "Manrope", sans-serif;
  text-align: center;
}

h1 {
  font-size: 24px;
  color: #ff4081;
}

h2 {
  font-size: 22px;
  color: #ff4081;
  margin-top: 20px;
}

.time {
  font-size: 20px;
  color: #555;
  font-weight: bold;
  margin: 10px 0;
}

.countdown {
  max-width: 400px;
  flex-direction: row;
  display: flex;
  font-size: 20px;
  font-weight: bold;
  color: white;
  background: linear-gradient(45deg, #ff4081, #ff80ab);
  display: inline-block;
  width: 100%;
  padding: 10px 20px;
  border-radius: 15px;
  box-shadow: 3px 3px 10px rgba(0, 0, 0, 0.2);
}

.countdown span {
  margin: 0 5px;
}
</style>
