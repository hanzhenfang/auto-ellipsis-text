# Auto Ellipsis Text For Vue3 Projects

## Install

 `npm i auto-ellipsis-text`

 `pnpm i auto-ellipsis-text`

 `yarn i auto-ellipsis-text`

## Basic Usage

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

      <!-- When you want to ellipsis the words on the "width" had be made sure.
       Below the parent element's width is 200px
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

![picture 1](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-215252302.png)  

## Additional options

There have two additional props help you unlock more useage scenarios.

  Sometime you may want to reserve the file surrfix infomathion, like :picture1.jpg、picture.png、music.mp4 etc...

The surrfix props can help you.

- **surrfix**:boolean

```vue
        <!-- The primitive words -->
        <span> somePictureInfomationYouWantToReserveTheSurrfix.jpg </span>

      <!-- When you want to ellipsis the words on the "width" had be made sure.
       Below the parent element's width is 200px
      -->
      <div class="w-200px">
        <AutoEllipsis :suffix="true">
          somePictureInfomationYouWantToReserveTheSurrfix.jpg
        </AutoEllipsis>
      </div>

```

![picture 2](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-215642827.png)  

Sometimes you may want when the height of the two rows then starts to ellipsis.You also can do it easily use the **startEllpsisLine** props.

- start-ellpsis-line:number

```vue
    <!-- The primitive words -->
        <span>
          somemePictureInfomationYouWantToReserveTheSurrfix.jpgsomePicture.jpg
        </span>
      </div>

      <!-- When you want to ellipsis the words on the "width" had be made sure.
       Below the parent element's width is 200px
      -->
      <div class="w-200px">
        <AutoEllipsis :start-elipsis-line="2">
          somePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpg
        </AutoEllipsis>
      </div>

```

![picture 3](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-220904001.png)  

And you can use the two props at the sametime.Like blow this.

```vue
  <AutoEllipsis :start-elipsis-line="2" :suffix="true">
          somePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpg
        </AutoEllipsis>
```

![picture 4](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-221425864.png)  
