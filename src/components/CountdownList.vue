<template>
  <div class="countdowns-container">
    <CountdownItem
        v-for="countdown in countdowns"
        :key="countdown.id"
        :countdown="countdown"
        @delete="deleteCountdown(countdown.id)"
    />

  </div>
</template>

<script>
  import CountdownItem from "./CountdownItem.vue"
  
  export default {
    components: {
      CountdownItem
    },
    props: {
      countdowns: Array
    },
    setup(_, { emit }) {
      const deleteCountdown = (id) => {
        emit("delete-countdown", id);
      }
      
      return {
        deleteCountdown
      }
    }
  }
</script>

<style>
  .countdowns-container {
    display: flex;
    flex-direction: column;
    width: 90%;
    margin-bottom: 20px;
    text-align: center;
  }
</style>