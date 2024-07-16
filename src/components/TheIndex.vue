<script>
import TheInventory from '../components/TheInventory.vue'
import AppSkeleton from '../components/AppSkeleton.vue'

export default {
  name: 'TheIndex',
  components: {
    TheInventory,
    AppSkeleton
  }
}
</script>

<template>
    <div class="container">
        <AppSkeleton img="../assets/Img.svg" title="ddd" text="fddddf"/>
        <TheInventory/>
        <AppSkeleton title="ddd" :closeBtn="true"/> 
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
  