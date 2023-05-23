<script lang="ts" setup>
import { ref, computed, nextTick, type Ref } from "vue";

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
const initStr = ref<HTMLSpanElement>(); // 用于调整文本
const initHeightStr = ref<HTMLSpanElement>(); // 用于计算文本初始高度
const cssEntirely = computed<boolean>(() => {
  return !props.suffix && props.startEllipsisLine === 1;
});

/* *
 * 总体的思路是根据实际高度去调整字体大小，防止溢出
 * 能够根据字体大小去调整字数
 * */ 

//开启保留后缀 自动调整文本字符数量
function adjustWordSuffix(endLeft: number, startRight: number, str: string, canFitHeight: number): string {
  initStr.value!.textContent = str.slice(0, endLeft + 1) + "..." + str.slice(startRight);
  const initStrHeight = Math.floor(getRefHeight(initStr));
  if (initStrHeight <= canFitHeight) { // 初始计算时 文本宽度溢出 情况
    return addWordSuffix(endLeft, startRight, str, canFitHeight);
  } else { // 容器有多余的位置
    return removeWordSuffix(endLeft, startRight, str, canFitHeight);
  }
}

// 容器有多余的位置，增加字符
function addWordSuffix(endLeft: number, startRight: number, str: string, canFitHeight: number): string {
  let canFiiled = true;
  while (canFiiled) { //交替增加
    canFiiled = false;
    initStr.value!.textContent = str.slice(0, endLeft + 2) + "..." + str.slice(startRight); // 左部末位往后走
    let tempHeight = Math.floor(getRefHeight(initStr));
    if (tempHeight <= canFitHeight) {
      endLeft++;
      canFiiled = true;
    }
    initStr.value!.textContent = str.slice(0, endLeft + 1) + "..." + str.slice(startRight - 1);// 右部首位往前走
    tempHeight = Math.floor(getRefHeight(initStr));
    if (tempHeight <= canFitHeight) {
      startRight--;
      canFiiled = true;
    }
  }
  return str.slice(0, endLeft + 1) + "..." + str.slice(startRight);
}

// 文本高度溢出 需减少字符
function removeWordSuffix(endLeft: number, startRight: number, str: string, canFitHeight: number): string {
  let canRemoved = true;
  while (canRemoved) {
    canRemoved = false;
    initStr.value!.textContent = str.slice(0, endLeft + 1) + "..." + str.slice(startRight);// 左部末位往前走
    let tempHeight = Math.floor(getRefHeight(initStr));
    if (tempHeight > canFitHeight) {
      endLeft--;
      canRemoved = true;
    }
    initStr.value!.textContent = str.slice(0, endLeft + 1) + "..." + str.slice(startRight);// 右部首位往后走
    tempHeight = Math.floor(getRefHeight(initStr));
    if (tempHeight > canFitHeight) {
      startRight++;
      canRemoved = true;
    }
  }
  return str.slice(0, endLeft + 1) + "..." + str.slice(startRight);
}

//未开启保留后缀  自动调整文本字符数量
function adjustWordNoSufiix(shouldDelNumber: number, str: string, canFitHeight: number): string {
  initStr.value!.textContent = str.slice(0, -shouldDelNumber) + "...";
  const initStrHeight = Math.floor(getRefHeight(initStr));
  let _continue = true;
  if (initStrHeight <= canFitHeight) { // 容器有多余的位置
    while (_continue) {
      _continue = false;
      initStr.value!.textContent = str.slice(0, -shouldDelNumber) + "...";
      let tempHeight = Math.floor(getRefHeight(initStr));
      if (tempHeight < canFitHeight) {// 末位往后走
        shouldDelNumber--;
        _continue = true;
      }
    }
  } else { // 初始计算时 文本高度溢出 情况
    while (_continue) {
      _continue = false;
      initStr.value!.textContent = str.slice(0, -shouldDelNumber) + "...";
      let tempHeight = Math.floor(getRefHeight(initStr));
      if (tempHeight > canFitHeight) {// 末位往前走
        shouldDelNumber++;
        _continue = true;
      }
    }
  }
  return str.slice(0, -shouldDelNumber) + "...";
}


//获取元素准确高度
function getRefHeight(ref: Ref): number {
  return Number(getComputedStyle(ref.value).height.slice(0, -2));
}
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
      strNumer - canFitStrNumber + props.boundaryValue;
    const delEachSide = shouldDelNumber / 2;
    const endLeft = Math.floor(strNumer / 2 - delEachSide);
    const startRight = Math.ceil(strNumer / 2 + delEachSide);
    initHeightStr.value!.textContent = str.slice(0, endLeft + 1) + "..." + str.slice(startRight);
    const canFitHeight = getRefHeight(initHeightStr);
    switch (props.suffix) {
      case true: {
        textNode.textContent = adjustWordSuffix(endLeft, startRight, str, canFitHeight * props.startEllipsisLine)
        break;
      }
      case false: {
        textNode.textContent = adjustWordNoSufiix(shouldDelNumber, str, canFitHeight * props.startEllipsisLine);
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
  <div style="overflow: hidden;width: 100%;" >
    <span ref="initStr"
    style="position: absolute; visibility: hidden;z-index: -9;word-break: break-all;white-space: normal;"></span>
  <span ref="initHeightStr" style="position: absolute; visibility: hidden;z-index: -9;white-space: nowrap;"></span>
  </div>
</template>
