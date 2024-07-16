<script setup>
import { defineProps, toRefs } from "vue";
const props = defineProps({
    img: String,
    title: String,
    text: String,
    closeBtn: Boolean,
})
const { img, title, text, closeBtn} = toRefs(props)
</script>
<template>
    <div class="sleketon_wrapper" :style="closeBtn && {paddingRight: '50px'}">
        <div v-if="closeBtn" class="closeBtn">&#10006;</div>
        <img v-if="img" src="../assets/Img.svg"/>
        <div v-if="title" class='title skeleton'></div>
        <div v-if="text" class='text'>
            <div class='skeleton'></div>
            <div class='skeleton'></div>
            <div class='skeleton'></div>
            <div class='skeleton'></div>
            <div class='skeleton'></div>
        </div>
    </div>
</template>

<style scoped lang="scss">
.sleketon_wrapper{
    position: relative;
    padding: 25px;
    display: flex;
    flex-direction: column;
    gap: 20px;
    align-items: center;
    background-color: #262626;
    border: 0.5px solid #3e3e3e;
    border-radius: 20px;
    .closeBtn{
        cursor: pointer;
        position: absolute;
        top: 10px;
        right: 15px;
        color: white;
    }
    img{
        width: 80%;
        filter: blur(4px);
        margin-bottom: 5px;
    }
    .title{
        width: 100%;
        height: 35px;
        border-radius: 12px;
        margin-bottom: 5px;
    }
    .text{
        width: 100%;
        display: flex;
        flex-direction: column;
        gap: 10px;
        div{
            width: 100%;
            height: 15px;
            border-radius: 10px;
        }
    }
}

.skeleton {
  animation: skeleton-loading 1s linear infinite alternate;
}

@keyframes skeleton-loading {
  0% {
    background-color: hsl(300, 2%, 29%);
  }
  100% {
    background-color: hsl(204, 3%, 62%);
  }
}
</style>