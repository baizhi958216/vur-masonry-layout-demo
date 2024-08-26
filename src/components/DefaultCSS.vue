<template>
  <div class="container">
    <div v-for="item in randomImgList" class="imgitems">
      <img :src="item.src" :height="item.height" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref } from "vue";

const imgSrc = "https://picsum.photos/360";
const height = ["400", "450", "420", "460", "480", "440"];

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

<style scoped>
.container {
  column-count: 5;
  column-gap: 0;
}

.imgitems {
  padding: 2px;
  position: relative;
  counter-increment: item-counter;
}

.imgitems::after {
  position: absolute;
  display: block;
  top: 2px;
  left: 2px;
  width: 24px;
  height: 24px;
  text-align: center;
  line-height: 24px;
  background-color: #000;
  color: #fff;
  content: counter(item-counter);
}

.imgitems img {
  display: block;
  width: 100%;
  height: auto;
}
</style>
