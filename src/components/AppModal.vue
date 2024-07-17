<script setup>
import { defineProps, defineEmits, toRefs, ref } from "vue";
const props = defineProps({
    item: {
            id: Number,
            name: String,
            count: Number,
            row: Number,
            col: Number,
            img: String,
    },
})
const emit = defineEmits(['updateItems','closeModal'])
const { item } = toRefs(props)
const isShowDeleteForm = ref(false);
const countInput = ref(null);
const deleteItem = () => {
    if (!isNaN(Number(countInput.value)) && countInput.value) {
        if (countInput.value > 0 && countInput.value <= item.value.count) {
            let itemsLocalStorage = JSON.parse(localStorage.getItem('items'));
            if (Number(countInput.value) === item.value.count) {
                itemsLocalStorage = itemsLocalStorage.filter(it => it.id !== item.value.id);
                emit('closeModal')
            }
            else {
                itemsLocalStorage.forEach(function(it, i) {
                    if(it.id == item.value.id) {
                        itemsLocalStorage[i] = { ...it, count: it.count-Number(countInput.value) }
                    }
                })
                emit('closeModal')
            }      
            localStorage.setItem('items', JSON.stringify(itemsLocalStorage));
            emit('updateItems', itemsLocalStorage);
        }
        else {
            window.alert('Недостаточное количество')
        }
    }
    else {
        window.alert('Введите число')
    }
}

</script>
<template>
    <div v-if="item" class="modal">
        <div class="closeBtn"><div @click="() => { emit('closeModal') }">&#10006;</div></div>
        <img :src="'images/inventoryItems/'+item.img+'.svg'"/>
        <div class="line"></div>
        <div class='title skeleton'></div>
        <div class='text'>
            <div class='skeleton'></div>
            <div class='skeleton'></div>
            <div class='skeleton'></div>
            <div class='skeleton'></div>
            <div class='skeleton'></div>
        </div>
        <div class="line"></div>
        <div class="deleteBtn" @click="isShowDeleteForm=true">Удалить предмет</div>
        <div class="deleteForm" :style="isShowDeleteForm ? 'display: flex; visibility: visible' : 'display: none; visibility: hidden'">
            <input v-model="countInput" placeholder="Введите количество"/>
            <div class="btns">
                <div class="canselBtn" @click="isShowDeleteForm=false">Отмена</div>
                <div class="confirmBtn" @click="deleteItem">Подтвердить</div>
            </div>
        </div>
    </div>
    
</template>

<style scoped lang="scss">
.modal{
    position: relative;
    height: 100%;
    width: 250px;
    position: relative;
    padding: 25px;
    display: flex;
    flex-direction: column;
    gap: 20px;
    align-items: center;
    background-color: #262626;
    border: 0.5px solid #3e3e3e;
    width: 250px;
    .line{
        width: 100%;
        border-top: 1px solid #3e3e3e;
    }
    .closeBtn{
        display: flex;
        padding-top: 10px;
        
        color: white;
        align-items: end;
        justify-content: end;
        text-align: end;
        width: 100%;
        div{
            cursor: pointer;
        }
    }
    img{
        height: 130px;
        width: 130px;
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
    .deleteBtn{
        cursor: pointer;
        border-radius: 10px;
        width: 100%;
        padding: 10px 0;
        display: flex;
        justify-content: center;
        background-color: #fa7272;
        color: white;
        &:hover {
            opacity: 0.8;
        }
    }
    .deleteForm{
        border: 1px solid #3e3e3e;
        border-bottom: none;
        padding: 20px 0;
        display: flex;
        flex-direction: column;
        gap: 20px;
        background-color: #262626;
        position: absolute;
        width: 100%;
        bottom: 0;
        will-change: max-height;
        overflow: hidden;
        transition: max-height 1s linear;
        display: flex;
        align-items: center;
        border-bottom-right-radius: 20px;
        input{
            background-color: #262626;
            padding: 7px; 
            border: 1px solid #3e3e3e;
            color: #7d7d7d;
            font-weight: 500;
        }
        .btns{
            display: flex;
            flex-direction: row;
            gap: 10px;
            padding-bottom: 20px;
            .canselBtn, .confirmBtn{
                border-radius: 10px;
                width: 100%;
                padding: 10px;
                display: flex;
                justify-content: center;
                cursor: pointer;
                &:hover {
                opacity: 0.8;
                }
            }
            .canselBtn{
                background-color: white;
                color: black;
            }
            .confirmBtn{
                background-color: #fa7272;
                color: white;
            }
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