<script setup lang="ts">
import { ref } from "vue";

import EllipsisText from "./components/EllipsisText.vue";

const testRowContent = [
  {
    content: "测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试",
    width: "200px",
    startEllipsis: 1,
    surrfix: false,
  },

  {
    content: "测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试",
    width: "400px",
    startEllipsis: 1,
    surrfix: true,
  },
  {
    content: "测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试",
    width: "300px",
    startEllipsis: 2,
    surrfix: true,
  },
];
</script>

<template>
  <div class="w-100vw h-100vh">
    <div class="w-full h-full flex pl-200px pt-200px">
      <div class="w-full h-full flex flex-col">
        <div
          v-for="(item, index) in testRowContent"
          class="border-#0000C4 border-y-10vh text"
          :style="{ maxWidth: item.width }"
        >
          {{ item.width }}
          <EllipsisText
            :start-elipsis-line="item.startEllipsis"
            :suffix="item.surrfix"
          >
            {{ item.content }}
          </EllipsisText>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
