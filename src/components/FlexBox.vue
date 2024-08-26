<template>
  <VueFlexWaterfall
    break-by-container
    col="5"
    align-content="center"
    col-spacing="0"
  >
    <!-- items -->
    <div style="padding: 2px" v-for="item in randomImgList">
      <img :src="item.src" style="height: 100%" width="266" />
    </div>
  </VueFlexWaterfall>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref } from "vue";
import { VueFlexWaterfall } from "vue-flex-waterfall";

const imgSrc = "https://picsum.photos/360";
const height = ["200", "350", "320", "260", "280", "240"];

const randomImgList = ref<
  {
    src: string;
    height: string;
  }[]
>([]);

// 服务器图片总数
const totalLength = ref(0);
// 模拟接口返回
const genImage = () => {
  for (let index = 0; index < 30; index++) {
    const index = ~~(Math.random() * height.length);
    randomImgList.value.push({
      src: imgSrc + "/" + height[index] + "?random=" + index,
      height: height[index],
    });
  }
  // 假设服务器只有120张图片
  totalLength.value = 120;
};

genImage();

onUnmounted(() => {
  window.removeEventListener("scroll", () => {});
});

onMounted(() => {
  window.addEventListener("scroll", () => {
    // 图片总数等于已加载总数的时候不再请求
    if (randomImgList.value.length >= totalLength.value) return;
    // 页面滚动到底部的时候调用genImage
    if (
      window.scrollY + window.innerHeight >=
      document.body.scrollHeight - 10
    ) {
      genImage();
    }
  });
});
</script>
