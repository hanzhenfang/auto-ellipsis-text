# Auto Ellipsis Text For Vue3 Projects

## Install

 `npm i auto-ellipsis-text`

 `pnpm i auto-ellipsis-text`

 `yarn i auto-ellipsis-text`

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

  Sometime you may want to reserve the file surrfix infomathion, like :picture1.jpg、picture.png、music.mp4 etc...

The surrfix props can help you.

- **surrfix**:boolean

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

Sometimes you may want when the height of the two rows then starts to ellipsis.You also can do it easily use the **startEllpsisLine** props.

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

The result will be like below this.

![picture 3](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-220904001.png)  

And you can use the two props at the sametime.Like blow this.

```vue
  <AutoEllipsis :start-ellipsis-line="2" :suffix="true">
          somePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpgsomePictureInfomationYouWantToReserveTheSurrfix.jpg
        </AutoEllipsis>
```

The result :

![picture 4](https://cdn.jsdelivr.net/gh/hanzhenfang/vite-vue-ts@master/README/IMG_20230319-221425864.png)  

## Auto Resize

The component will auto calculate the number of words in the containner if the browser window is resized, too!

## License

MIT © hanzhenfang
