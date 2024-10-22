<template>
  <div class="container">
    <CountdownForm @add-countdown="addCountdown"/>
    
    <CountdownList :countdowns="countdowns" @delete-countdown="deleteCountdown"/>
  </div>
</template>

<script>
    import { ref } from 'vue';
    
    import CountdownForm from "./components/CountdownForm.vue"
    import CountdownList from "./components/CountdownList.vue"
    
    export default {
      components: {
        CountdownForm,
        CountdownList
      },
      setup() {
        const countdowns = ref([]);
        
        const addCountdown = (countdown) => {
          countdowns.value.push(countdown);
        }
        
        const deleteCountdown = (id) => {
          const index = countdowns.value.findIndex(c => c.id === id);
          if (index !== -1) countdowns.value.splice(index, 1);
        }
        
        return {
          countdowns,
          addCountdown,
          deleteCountdown
        }
      }
    }
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 400px;
    margin: 0 auto;
    padding: 20px;
    border-radius: 12px;
    background: rgba(255, 255, 255, 0.9);
    color: black;
  }
</style>
