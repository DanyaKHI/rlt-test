<script setup>
import TheInventoryItem from '../components/TheInventoryItem.vue'
import AppModal from '../components/AppModal.vue'
import { ref, onMounted} from "vue";


const items = ref([]);
const selectedItem = ref(null);
const modal = ref(null);
const isShowModal = ref(false);

onMounted(() => {
    let itemsLocalStorage = localStorage.getItem('items');
    if (itemsLocalStorage) {
        items.value = JSON.parse(itemsLocalStorage);
    }
    else {
        fetch('data.json')
        .then(res => res.json())
        .then(data => {
            items.value = data.items
            localStorage.setItem('items', JSON.stringify(data.items));
        })
    }
});

const clickItemInventory = (e, item) => {
    if (!isShowModal.value || (selectedItem.value && isShowModal.value)) {
        isShowModal.value = true
        selectedItem.value = item
    }
    else {
        isShowModal.value = false
        selectedItem.value = null
    }
}
</script>

<template>
    <div class="inventory__wrapper">
        <div class="inventory">
            <div class="inventory__row" v-for="row in 5" :key="row"> 
                <div class="inventory__col" v-for="col in 5" :key="col">
                    <TheInventoryItem 
                    :item = "items.find(item => item.row===row && item.col === col)" 
                    :col="col" 
                    :row="row" 
                    @clickItem="(e) => { clickItemInventory(e, items.find(item => item.row===row && item.col === col)) }"
                    @updateItems="items = $event"/>
                </div>
            </div>
        </div>
        <div
        ref="modal"
        class='modal__wrapper'
        :style="isShowModal ? 'max-width: 600px' : 'max-width:0'">
            <AppModal 
            :item="selectedItem" 
            @closeModal="isShowModal = !isShowModal" 
            @updateItems="items = $event"/>
        </div>
    </div>
</template>
    
<style scoped lang="scss">
.inventory__wrapper{
    position: relative;  

    .inventory{
        height: 100%;
        position: relative;
        border-radius: 20px;
        background-color: #262626;
        display: grid;
        grid-template-rows: repeat(5, 1fr);
        overflow: hidden;
        border: 0.5px solid #3e3e3e;
        
        &__row{
            display: grid;
            grid-template-columns: repeat(5, 1fr);
        }
        &__col{
            border: 0.5px solid #3e3e3e;
        }
    }
    .modal__wrapper{
        height: 100%;
        position: absolute;
        top: 0;
        right: 0;
        will-change: max-width;
        overflow: hidden;
        transition: max-width 1s linear;
        display: flex;
        align-items: center;
        border-top-right-radius: 20px;
        border-bottom-right-radius: 20px;
    }
}
</style>
    