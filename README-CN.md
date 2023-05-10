# Auto Ellipsis Text For Vue3 Projects

## 安装

 `npm i auto-ellipsis-text`

 `pnpm i auto-ellipsis-text`

 `yarn add auto-ellipsis-text`

## 基本用法

```vue
import {AutoEllipsis} from "auto-ellipsis-text"
```
引入以后，你可以用`<AutoEllipsis>`把文字包起来。 它会自动计算父元素的宽度, 并自动计算`<AutoEllipsis>`中可容纳的最大字符数量。

```vue

<script setup lang="ts">
import { AutoEllipsis } from "auto-ellipsis-text";
</script>

<template>
  <div class="w-100vw h-100vh text-14px text-black">
    <div class="w-full h-full overflow-hidden pl-200px pt-200px">
      <div>
        <!-- 原始的字符 -->
        <span> Somelongword_Somelongword_Somelongword_Somelongword_ </span>
      </div>

      <!-- 当你想在一个宽度已经确定的容器里使文字呈现省略效果
      比如下面这个容器的宽度是200px
      -->
      <div class="w-200px">
        <AutoEllipsis>
          Somelongword_Somelongword_Somelongword_Somelongword_
        </AutoEllipsis>
      </div>
    </div>
  </div>
</template>
```

渲染出来的结果长这样。

![picture 1](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-215252302.png)  

## 可选的参数

这里有两个可选的参数能帮你解锁更多的想法。

  有时你可能会想要保留文件的后缀信息, 比如 :picture1.jpg、picture.png、music.mp4之类的……

`suffix`参数可以帮到你。

- **suffix**:boolean

```vue
        <span> somePictureInfomationYouWantToReserveTheSurrfix.jpg </span>
      <div class="w-200px">
        <AutoEllipsis :suffix="true">
          somePictureInfomationYouWantToReserveTheSurrfix.jpg
        </AutoEllipsis>
      </div>

```

渲染出来的结果长这样。

![picture 2](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-215642827.png)  

有时你可能会希望实现从第二行开始省略的效果。那么你可以使用`start-ellipsis-line`参数来轻松做到这一点。

- start-ellpsis-line:number

```vue
        <span>
          somemePictureInfomationYouWantToReserveTheSurrfix.jpgsomePicture.jpg
        </span>
      </div>
      <div class="w-200px">
        <AutoEllipsis :start-ellipsis-line="2">
          somePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpg
        </AutoEllipsis>
      </div>

```

渲染出来的结果长这样。

![picture 3](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-220904001.png)  

你可以同时使用上述两个参数，就像下面这样。

```vue
  <AutoEllipsis :start-ellipsis-line="2" :suffix="true">
          somePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpg
        </AutoEllipsis>
```

渲染出来的结果长这样。

![picture 4](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-221425864.png)  

## Bugs

new props(2023/5/8) => boundaryValue:number

这个组件目前存在着一个bug，由于汉字、数字或特殊符号的宽度，导致渲染出来的结果可能会混乱。

![picture 5](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230508-191257047.png)  

为了暂时解决这个问题，我新增了一个参数让用户可以手动增加被删除的字符数量。这里是使用`boundaryValue`参数的例子。

```vue
        <AutoEllipsis
          :start-ellipsis-line="2"
          :suffix="true"
          :boundary-value="1"
        >
          SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_.jpg
        </AutoEllipsis>
```

渲染出来的结果长这样。

![picture 6](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230508-191540600.png)

## Auto Resize

如果浏览器窗口发生了resize，这个组件也会自动计算容器里的字符数量。

## License

MIT © hanzhenfang
