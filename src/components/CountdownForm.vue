<template>
  <div class="input-container" id="input-container">
    <form action="#" class="countdown-form">
      <label for="event-title">Tytuł wydarzenia:</label>
      <input v-model="eventTitle" id="event-title" type="text" placeholder="Tytuł wydarzenia">
      
      <label for="date-picker">Wybierz datę:</label>
      <input v-model="date" id="date-picker" :min="minDate" type="datetime-local">
      
      <button @click="createCountdown" id="new-countdown">Dodaj</button>
    </form>
  </div>
</template>

<script>
  import { ref } from 'vue';
  
  export default {
    setup(_, {emit}) {
      const eventTitle = ref("");
      const date = ref("");
      const minDate = ref(new Date().toISOString().slice(0, -8));
      
      const createCountdown = () => {
        if (!eventTitle.value || !date.value) return;
        
        emit("add-countdown", {
          title: eventTitle.value,
          date: new Date(date.value),
          id: Date.now()
        });
        
        eventTitle.value = "";
        date.value = "";
      }
      
      return {
        eventTitle,
        date,
        minDate,
        createCountdown
      };
    }
  }
</script>

<style>
  .input-container label {
    font-weight: bold;
  }
  
  .input-container input {
    margin: 5px auto 10px;
    padding: 10px;
    width: 95%;
    border: 1px solid #ccc;
    background: #fff;
    outline: none;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
  }
  
  .input-container #new-countdown {
    width: 100%;
    height: 40px;
    border-radius: 10px;
    margin-top: 15px;
    border: none;
    background: #006959;
    color: #fff;
    cursor: pointer;
    outline: none;
    text-transform: uppercase;
  }
  
  .input-container #new-countdown:hover {
    filter: brightness(140%);
  }
</style>