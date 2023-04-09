<script setup lang="ts">
import ResponsivePicture from '@/components/ResponsivePicture.vue'
import GrideLayout from '@/components/GrideLayout.vue'
import FromCom from '@/components/FormCom.vue'
import { onMounted } from 'vue'
onMounted(() => {
    duplicateRemoval()
})
/**
 * 4、数组去重
 */
const duplicateRemoval = function () {
    let arr = [1, 2, 3, 3, 2, 'cc', 'cc'];
    let set = new Set(arr); //set()数组去重之后变成类数组
    console.log(Array.from(set)); //Array.from()将它变为数组
}
// 5、栈
// ①基于数组
class ArrayStack {
    items: any[] = [];
    constructor() {
        this.items = []
    }
    print() {
        return this.items
    }
    // 栈内加元素
    push(item: any) {
        this.items.push(item)
    }
    // 取栈顶元素
    peek() {
        return this.items[this.items.length - 1]
    }
    // 出栈
    pop() {
        return this.items.pop()
    }
    size(): number {
        return this.items.length
    }
    isEmpty(): boolean {
        return this.items.length == 0
    }
    clear(): void {
        this.items = []
    }
}
// ②基于对象,使用对象来模拟栈，可以解决数组中的方法都需要遍历数组的情况，理论上来说是要比数组实现的栈效率更高
class ObjectStack {
    items = {};
    count: number = 0
    constructor() {
        this.items = {}
        this.count = 0
    }
    print() {
        return this.items
    }
    push(item: any) {
        this.items[this.count++] = item
    }
    peek() {
        if (this.isEmpty()) return undefined
        return this.items[this.count - 1]
    }
    pop() {
        if (this.isEmpty()) return undefined
        this.count--
        const res = this.items[this.count]
        delete this.items[this.count]
        return res
    }
    size(): number {
        return Object.keys(this.items).length
    }
    isEmpty(): boolean {
        return this.count == 0
    }
    clear(): void {
        this.items = {}
        this.count = 0
    }
}
</script>
<template>
    <div class="home">
        <h1>1、</h1>
        <responsive-picture></responsive-picture>
        <h1>2、</h1>
        <gride-layout></gride-layout>
        <br>
        <h1>3、</h1>
        <from-com></from-com>
    </div>
</template>
<style scoped>
.home {
    display: flex;
    flex-direction: column;
    align-items: center;
}
</style>
