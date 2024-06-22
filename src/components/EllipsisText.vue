<script lang="ts" setup>
import { ref, computed, nextTick } from "vue";

const props = withDefaults(
  defineProps<{
    suffix?: boolean;
    startEllipsisLine?: number;
    boundaryValue?: number;
    rootDivId?: string;
  }>(),
  {
    startEllipsisLine: 1,
    suffix: false,
    boundaryValue: 0,
    rootDivId: "app",
  }
);

let primitiveText: string = "";
const container = ref<HTMLDivElement>();
const text = ref<HTMLSpanElement>();
const cssOnly = computed<boolean>(() => {
  return !props.suffix && props.startEllipsisLine === 1;
});

function autoElipsis(container: HTMLElement, textNode: HTMLSpanElement) {
  const str = primitiveText;
  textNode.textContent = str;
  container.style.whiteSpace = "nowrap";
  container.style.width = "fit-content";
  const containerWidth = container.clientWidth;
  const parent = container.parentElement; // outer parents element
  if (!parent) return;
  const parentWidth = parent.clientWidth || parent.offsetWidth;

  const fontSize = getComputedStyle(textNode)["fontSize"];
  textNode.style.height = parseFloat(fontSize) * props.startEllipsisLine + "px";

  const ultimatelyHeight = container.clientHeight * props.startEllipsisLine; // use to estimate the finally result in startEllipsisLine prop is greater than 1

  function justCssfn() {
    container.style.width = parentWidth + "px";
    container.style.whiteSpace = "nowrap";
    container.style.textOverflow = "ellipsis";
    container.style.overflow = "hidden";
  }

  if (parentWidth <= 0)
    throw new Error(
      "please wrap this component in a spec width father element.(请将该元素放置在一个指定准确宽度的父元素内部使用)"
    );
  if (containerWidth <= parentWidth) {
    textNode.textContent = str;
    return;
  } else if (cssOnly.value) {
    justCssfn();
    return;
  } else {
    const textWidth = textNode.offsetWidth;
    const strNumer = str.length;
    const avgStrWidth = textWidth / strNumer;
    const canFitStrNumber = Math.floor(
      (parentWidth * props.startEllipsisLine) / avgStrWidth
    );

    if (strNumer - canFitStrNumber >= 0) {
      let flag = props.boundaryValue;
      let containe_height = 0;
      do {
        const shouldDelNumber = strNumer - canFitStrNumber + 1.5 + flag;
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
        containe_height = container.clientHeight;
        flag++;
      } while (containe_height > ultimatelyHeight);
    } else {
      justCssfn();
    }
  }
}

const observer = new ResizeObserver(() => {
  if (!container.value) return;
  autoElipsis(container.value, text.value!);
});

nextTick(() => {
  if (!container.value) return;
  primitiveText = text.value?.innerText ?? "";
  if (primitiveText === "") return;
  const app = document.getElementById(props.rootDivId);
  if (!app) autoElipsis(container.value, text.value!);
  else observer.observe(app);
});
</script>
<template>
  <div ref="container" v-bind="$attrs">
    <span ref="text">
      <slot />
    </span>
  </div>
</template>
