<template>
  <div :class="{ 'countdown-finished': countdown.finished, 'countdown': true}">
    <h3 class="countdown-title">{{ countdown.title }}</h3>
    <div class="countdown-body" v-if="!countdown.finished">
      <ul class="timer">
        <li><span class="days"> {{ countdown.days }}</span>Dni</li>
        <li><span class="hours"> {{ countdown.hours }}</span>Godzin</li>
        <li><span class="minutes"> {{ countdown.minutes }}</span>Minut</li>
        <li><span class="seconds"> {{ countdown.seconds }}</span>Sekund</li>
      </ul>
    </div>
    <div v-else class="countdown-finished-info">
      Odliczanie zakończone!
    </div>
    <button class="countdown-delete" @click="deleteCountdown">X</button>
  </div>
</template>

<script>
  import { ref, onMounted, onUnmounted } from "vue";
  
  export default {
    props: {
      countdown: Object
    },
    setup(props, {emit}) {
      const countdownRef = ref(props.countdown);
      countdownRef.value.finished = false;

      const deleteCountdown = () => {
        emit("delete", countdownRef.value.id);
      }

      const calculateTimeLeft = () => {
        const now = new Date().getTime();

        const distance = countdownRef.value.date - now;

        if (distance > 0) {
          countdownRef.value.days = Math.floor(distance / (1000 * 60 * 60 * 24));

          countdownRef.value.hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));

          countdownRef.value.minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));

          countdownRef.value.seconds = Math.floor((distance % (1000 * 60)) / 1000);
        } else {
          countdownRef.value.finished = true;
        }
      }

      const intervalId = setInterval(calculateTimeLeft, 1000);
      onMounted(calculateTimeLeft);
      onUnmounted(() => {
        clearInterval(intervalId);
      });
      
      return {
        countdownRef,
        deleteCountdown
      }
    }
  }
</script>

<style>
  .countdown {
    position: relative;
    margin-top: 5px;
    padding: 5px;
  }
  
  .countdown-title {
    margin: 0;
    padding: 0;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  
  .countdown-body {
    display: flex;
    justify-content: center;
    border-bottom: 1px dotted black;
  }
  
  .countdown-finished {
    background-color: #02aab0;
    border-radius: 5px;
    color: white;
  }
  
  ul {
    margin: 0;
    padding: 0;
  }
  
  ul li {
    display: inline-block;
    padding: 5px;
    font-size: 1rem;
    text-transform: uppercase;
    list-style-type: none;
  }
  
  ul li span {
    display: block;
    text-align: center;
  }
  
  .timer.hide {
    display: none;
  }
  
  .countdown-finished-info {
    font-weight: bold;
  }
  
  .countdown-finished-info.show {
    display: block;
  }
  
  button.countdown-delete {
    position: absolute;
    top: 10px;
    right: 10px;
    display: block;
    height: 20px;
    background-color: inherit;
    cursor: pointer;
    border: none;
    border-radius: 4px;
    font-weight: bold;
  }

  button.countdown-delete:hover {
    background-color: red;
  }
</style>