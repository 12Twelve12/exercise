<script setup lang="ts">
const btns = [
    { "name": "施耐德", "selected": true },
    { "name": "西门子", "selected": false },
    { "name": "ABB", "selected": false },
    { "name": "常熟", "selected": false },
    { "name": "上海人民", "selected": false },
    { "name": "德力西", "selected": true },
    { "name": "天正电气", "selected": false },
    { "name": "环宇", "selected": false },
    { "name": "杭电电气", "selected": false },
    { "name": "上海华通", "selected": false },
    { "name": "伊顿", "selected": true },
    { "name": "罗格朗", "selected": false },
    { "name": "凯帆", "selected": false },
    { "name": "天逸", "selected": false }
];
</script>
<template>
    <div class="grid-container">
        <div class="grid-item" :class="item.selected ? 'selected' : ''" v-for="item in btns" :key="item.name">{{ item.name
        }}
        </div>
    </div>
</template>
<style scoped>
.grid-container {
    display: grid;
    /* grid-template-columns: auto auto auto; */
    grid-template-columns: 120px 120px 120px;
    grid-gap: 10px 10px;
}

.grid-item {
    background-color: rgb(248, 248, 248);
    border-radius: 50px;
    width: 120px;
    height: 40px;
    text-align: center;
    line-height: 40px;
    color: rgb(139, 139, 139);
    font-size: 14px;
    cursor: pointer;

}

.selected {
    background-color: rgb(238, 251, 251);
    border: 1px solid rgb(78, 211, 218);
    color: rgb(78, 211, 218);
}
</style>