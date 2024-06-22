# Auto Ellipsis Text For Vue3 Projects

[English doc](./README.md)

[中文文档](./README-CN.md)

## Install

`npm i auto-ellipsis-text`

`pnpm i auto-ellipsis-text`

`yarn add auto-ellipsis-text`

## Basic Usage

```vue
import {AutoEllipsis} from "auto-ellipsis-text"
```

Once imported, you can then wrap any words with `<AutoEllipsis>`. This will automatic compute the width of the surrounding parent node, and auto calculate the max number of words can fit in the `<AutoEllipsis>`.

```vue
<script setup lang="ts">
import { AutoEllipsis } from "auto-ellipsis-text";
</script>

<template>
  <div class="w-100vw h-100vh text-14px text-black">
    <div class="w-full h-full overflow-hidden pl-200px pt-200px">
      <div>
        <!-- The primitive words -->
        <span> Somelongword_Somelongword_Somelongword_Somelongword_ </span>
      </div>

      <!-- When you want to ellipsis the words on a "width" had be made sure container.
      Just like the Below element that width is 200px
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

The result will be like below this.

![picture 1](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-215252302.png)

## Additional options

There have two additional props help you unlock more useage scenarios.

Sometime you may want to keep the file suffix, like :picture1.jpg、picture.png、music.mp4 etc...

The suffix props can help you.

- **suffix**:boolean

```vue
<span> somePictureInfomationYouWantToReserveTheSurrfix.jpg </span>
<div class="w-200px">
        <AutoEllipsis :suffix="true">
          somePictureInfomationYouWantToReserveTheSurrfix.jpg
        </AutoEllipsis>
      </div>
```

The result will be like below this.

![picture 2](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-215642827.png)

Sometimes you may want to see the ellipsis from the second line.You can do it easily by using the **start-ellipsis-line** props.

- start-ellipsis-line:number

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

The result will be like below this.

![picture 3](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-220904001.png)

And you can use both of this two props at the same time. Like below.

```vue
<AutoEllipsis :start-ellipsis-line="2" :suffix="true">
          somePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpg
        </AutoEllipsis>
```

The result :

![picture 4](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-221425864.png)

## Bugs

new props(2023/5/8) => boundaryValue:number

This component currently has a bug that causes the page to appear confused due to the width of Chinese characters 、 numbers or special symbol.

![picture 5](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230508-191257047.png)

To temporarily solve this problem, I added a props to let the user manually enter a number to increase the number to be deleted.Here's how to use the "boundaryValue" props.

```vue
<AutoEllipsis :start-ellipsis-line="2" :suffix="true" :boundary-value="1">
          SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_SomelongWord_.jpg
        </AutoEllipsis>
```

The result

![picture 6](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230508-191540600.png)

## Auto Resize

The component will auto calculate the number of words in the container if the browser window is resized, too!

## License

MIT © hanzhenfang
