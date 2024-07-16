<script>
import TheInventory from '../components/TheInventory.vue'
  export default {
  name: 'TheIndex',
  components: {
    TheInventory,
  }
  }
</script>

<template>
    <div class="container">
        <div>s</div>
        <TheInventory/>
        <div>s</div>  
    </div>
</template>
  
<style scoped lang="scss">
  .container{
    width: 70%;
    height: 100vh;
    box-sizing: border-box;
    padding: 40px;
    background-color: black;
    display: grid;
    grid-template-columns: 1fr 2fr;
    grid-template-rows: 3fr 0.5fr;
    gap: 40px;
    div:last-child{
      grid-column-start: 1;
      grid-column-end: 3;
    }
  }
</style>
  