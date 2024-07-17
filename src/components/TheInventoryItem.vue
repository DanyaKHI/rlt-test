<script setup>
import { defineProps, defineEmits, toRefs } from "vue";
const props = defineProps({
    item: {
            id: Number,
            name: String,
            count: Number,
            row: Number,
            col: Number,
            img: String,
    },
    col: Number,
    row: Number,
})
const { item, col, row } = toRefs(props)
const emit = defineEmits(['updateItems','clickItem'])


const drag = (ev) => {
    ev.dataTransfer.setData('text/plain', JSON.stringify(item.value))
};

const allowDrop = (ev) => {
    ev.preventDefault();
}

const drop = (ev) => {
    ev.preventDefault();
    let data = ev.dataTransfer.getData("text/plain");
    if (!item.value) {
        saveData(JSON.parse(data));
    }
    ev.dataTransfer.clearData();
    
}

const saveData = (data) => {
    data.col = col.value;
    data.row = row.value;
    let itemsLocalStorage = JSON.parse(localStorage.getItem('items'));
    itemsLocalStorage.forEach(function(item, i) {
        if(item.id == data.id) {
            itemsLocalStorage[i] = data
        }
    })
    localStorage.setItem('items', JSON.stringify(itemsLocalStorage));
    emit('updateItems', itemsLocalStorage);
  }
</script>

<template>
    <div class="item_wrapper" @drop="drop" @dragover="allowDrop">
        <div class="item" v-if="item" :id="item.id" @click="() => { emit('clickItem') }">
            <img class="item__img" :src="'images/inventoryItems/'+item.img+'.svg'" :id="item.id" @dragstart="drag"/>
            <div class="item__count">{{item.count}}</div>
        </div>
    </div>
</template>
    
<style scoped lang="scss">
.item_wrapper{
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    position: relative;
    .item{
        cursor: move;
        &__count{
        position: absolute;
        right: 0;
        bottom: 0;
        padding: 6px;
        color: #7d7d7d;
        border-top-left-radius: 10px 10px;
        border-top: 1px solid #3e3e3e;
        border-left: 1px solid #3e3e3e;
        }
    }
}
.item_wrapper:has(.item):hover {
    opacity: 0.5;
}
</style>
  