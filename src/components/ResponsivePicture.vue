<script setup lang="ts">
import { onMounted } from 'vue'
onMounted(() => {
    window.addEventListener("resize", () => {
        const width = document.body.clientWidth
        const dpr = window.devicePixelRatio
        console.log("当前宽度：", width, "，当前像素比：", dpr)
    })

})
</script>
<template>
    <!-- 分辨率切换问题 
    1、分辨率切换：不同的尺寸 
    srcset 定义了浏览器可选择的图片设置以及每个图片的大小
    srcset="指定图片 图片固有的宽度（即图片真实大小）"
    sizes 定义了一组媒体条件（例如屏幕宽度）并且指明当某些媒体条件为真时，什么样的图片尺寸是最佳选择
    sizes="一个媒体条件(当视口的宽度小于等于 600px 时) 当媒体条件为真时，图像将填充的槽的宽度（480px）,否则的话宽度为880px"
    步骤：
    ①检查设备宽度
    ②检查 sizes 列表中哪个媒体条件是第一个为真
    ③查看给予该媒体查询的槽大小
    ④加载 srcset 列表中引用的最接近所选的槽大小的图像
-->
    <img srcset="/img/480w.jpg 480w, /img/880w.jpg 880w" sizes="(max-width: 600px) 480px,880px" src="/img/880w.jpg" />
    <!-- 
    2、相同的尺寸，不同的分辨率
    如果用一个设备像素表示一个 CSS 像素就加载480w的图片，一点五个就加载680w的图片，两个就加载880w的图片 
    -->
    <img srcset="/img/480w.jpg, /img/680w.jpg 1.5x, /img/880w.jpg 2x" src="/img/680w.jpg" style="width: 320px;" />

    <!--美术设计问题
    如果视窗的宽度为 799px 或更少，第一个 <source> 元素的图片就会显示。如果视窗的宽度是 800px 或更大，就显示第二张图片。
    当媒体条件都不返回真的时候，它会显示默认图片。
     -->
    <picture>
        <source media="(max-width: 799px)" srcset="/img/880-mini.jpg" />
        <source media="(min-width: 800px)" srcset="/img/880w.jpg" />
        <img src="/img/880w.jpg" />
    </picture>
</template>
<style scoped></style>
