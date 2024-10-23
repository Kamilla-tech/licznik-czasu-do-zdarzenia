<template>
  <div>

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
  
</style>