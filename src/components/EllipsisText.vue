<script lang="ts" setup>
import { ref, computed, nextTick } from "vue";

const props = withDefaults(
  defineProps<{
    suffix?: boolean;
    startEllipsisLine?: number;
    boundaryValue?: number;
  }>(),
  {
    startEllipsisLine: 1,
    suffix: false,
    boundaryValue: 0,
  }
);

let premitiveText: string = "";
const container = ref<HTMLDivElement>();
const text = ref<HTMLSpanElement>();
const cssEntirely = computed<boolean>(() => {
  return !props.suffix && props.startEllipsisLine === 1;
});

function autoElipsis(container: HTMLElement, textNode: HTMLSpanElement) {
  const str = premitiveText;
  textNode.textContent = str;
  container.style.whiteSpace = "nowrap";
  container.style.width = "fit-content";
  const containerWidth = container.clientWidth;
  const parent = container.parentElement; // outer parents element
  const parentWidth = parent!.clientWidth || parent!.offsetWidth;
  if (containerWidth <= parentWidth) {
    textNode.textContent = str;
    return;
  } else if (cssEntirely.value) {
    container.style.width = parentWidth + "px";
    container.style.whiteSpace = "nowrap";
    container.style.textOverflow = "ellipsis";
    container.style.overflow = "hidden";
    return;
  } else {
    const textWidth = textNode.offsetWidth;
    const strNumer = str.length;
    const avgStrWidth = textWidth / strNumer;
    const canFitStrNumber = Math.floor(
      (parentWidth * props.startEllipsisLine) / avgStrWidth
    );
    const shouldDelNumber =
      strNumer - canFitStrNumber + 1.5 + props.boundaryValue;
    const delEachSide = shouldDelNumber / 2;
    const endLeft = Math.floor(strNumer / 2 - delEachSide);
    const startRight = Math.ceil(strNumer / 2 + delEachSide);

    switch (props.suffix) {
      case true: {
        textNode.textContent =
          str.slice(0, endLeft) + "..." + str.slice(startRight);
        break;
      }
      case false: {
        textNode.textContent = str.slice(0, -shouldDelNumber) + "...";

        break;
      }
    }
    container.style.wordBreak = "break-all";
    container.style.whiteSpace = "normal";
  }
}

const observer = new ResizeObserver(() => {
  const containerEl: HTMLDivElement =
    container.value! || document.getElementById("autoEllipsisWrapper");
  autoElipsis(containerEl, text.value!);
});

nextTick(() => {
  premitiveText = text.value?.innerText ?? "";
  const app = document.getElementById("app");
  observer.observe(app!);
});
</script>
<template>
  <div id="autoEllipsisWrapper" ref="container" v-bind="$attrs">
    <span ref="text">
      <slot />
    </span>
  </div>
</template>
