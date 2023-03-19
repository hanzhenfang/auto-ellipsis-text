<script lang="ts" setup>
import { ref, nextTick } from "vue";

const props = withDefaults(
  defineProps<{
    suffix?: boolean;
    startElipsisLine?: number;
  }>(),
  {
    startElipsisLine: 2,
    suffix: true,
  }
);

const container = ref<HTMLDivElement>();
const text = ref<HTMLSpanElement>();
let premitiveText: string = "";

function autoElipsis(
  container: HTMLElement,
  textNode: HTMLSpanElement,
  suffix: number = 0,
  startElipsisLine: number = props.startElipsisLine
) {
  if (!container || !textNode) return;
  const str = premitiveText;
  textNode.textContent = str;
  container.style.whiteSpace = "nowrap";
  const containerWidth = container.clientWidth;
  const parent = container.parentElement; // outer parents element
  const parentWidth = parent!.clientWidth || parent!.offsetWidth;
  if (containerWidth <= parentWidth) {
    textNode.textContent = str;
    return;
  } else {
    const textWidth = textNode.offsetWidth;
    const strNumer = str.length;
    const avgStrWidth = textWidth / strNumer;
    const canFitStrNumber = Math.floor(
      (parentWidth * startElipsisLine) / avgStrWidth
    );
    const shouldDelNumber = strNumer - canFitStrNumber + 4;
    const delEachSide = shouldDelNumber / 2;
    const endLeft = Math.floor(strNumer / 2 - delEachSide);
    const startRight = Math.ceil(strNumer / 2 + delEachSide);

    const _endLeft = endLeft - suffix > 1 ? endLeft - suffix : endLeft;
    const _startRight =
      startRight - suffix > suffix ? startRight - suffix : startRight;

    switch (props.suffix) {
      case true: {
        textNode.textContent =
          str.slice(0, _endLeft) + "..." + str.slice(_startRight);
        container.style.whiteSpace = "normal";
        break;
      }
      case false: {
        textNode.textContent = str.slice(0, -shouldDelNumber) + "...";
        container.style.whiteSpace = "normal";
        break;
      }
    }
  }
}

const observer = new ResizeObserver(() => {
  autoElipsis(container.value!, text.value!);
});

nextTick(() => {
  premitiveText = text.value?.innerText ?? "";
  const app = document.getElementById("app");
  observer.observe(app!);
});
</script>
<template>
  <div ref="container" :class="$attrs">
    <span ref="text">
      <slot> write some long words </slot>
    </span>
  </div>
</template>

<style scoped>
.container {
  width: fit-content;
  word-break: break-all;
}
</style>
