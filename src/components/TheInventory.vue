<script setup>
import TheInventoryItem from '../components/TheInventoryItem.vue'
import { ref, onMounted} from "vue";


const items = ref([]);
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
</script>

<template>
        <div class="inventory" >
            <div class="inventory__row" v-for="row in 5" :key="row"> 
                <div class="inventory__col" v-for="col in 5" :key="col">
                    <TheInventoryItem 
                    :item = "items.find(item=>item.row===row && item.col === col)" 
                    :col="col" 
                    :row="row" 
                    @updateItems="items = $event"/>
                </div>
            </div>
        </div>
</template>
    
<style scoped lang="scss">
    .inventory{
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
</style>
    