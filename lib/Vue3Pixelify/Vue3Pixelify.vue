<script setup lang="ts">
import { ref, onUpdated, onMounted, defineComponent } from 'vue'
import type { Ref } from 'vue'

interface Props {
  src: string
  pixelSize: number
  width?: number
  height?: number
  centered?: boolean
  fillTransparencyColor: string
}

const props = withDefaults(defineProps<Props>(), {
  fillTransparencyColor: 'white',
  pixelSize: 4
})

const canvas: Ref<HTMLCanvasElement> = ref(document.createElement('canvas'))

onMounted(() => {
  pixelify({
    src: props.src,
    pixelSize: props.pixelSize,
    width: props.width,
    height: props.height,
    centered: props.centered,
    fillTransparencyColor: props.fillTransparencyColor,
  })
})

onUpdated(() => {
  pixelify({
    src: props.src,
    pixelSize: props.pixelSize,
    width: props.width,
    height: props.height,
    centered: props.centered,
    fillTransparencyColor: props.fillTransparencyColor,
  })
})

function pixelify(
  { src, 
    width, 
    height, 
    pixelSize, 
    centered, 
    fillTransparencyColor 
  }: 
  { 
    src: Props['src'],
    width: Props['width'],
    height: Props['height'],
    pixelSize: Props['pixelSize'],
    centered: Props['centered'],
    fillTransparencyColor: Props['fillTransparencyColor'],
  }
) {
  pixelSize = Math.round(props.pixelSize);
  // create img that will be later painted into the canvas
  let img: HTMLImageElement = new Image();
  img.crossOrigin = "anonymous";
  img.src = src;
  // once image is loaded..
  img.onload = () => {
    const canv = canvas.value;
    const ctx = canv.getContext("2d");
    img.width = width ? width : img.width;
    img.height = height ? height : img.height;
    canv.width = +(img.width);
    canv.height = +(img.height);
    // we paint the image into the canvas
    // this is needed to get RGBA info out of each pixel
    ctx?.drawImage(img as CanvasImageSource, 0, 0, img.width, img.height);
    ctx && paintPixels({ ctx, img, pixelSize, centered, fillTransparencyColor });
    img = new Image();
  }
}

function paintPixels({
  ctx, 
  img, 
  pixelSize, 
  centered, 
  fillTransparencyColor
} : {
  ctx: CanvasRenderingContext2D,
  img: HTMLImageElement,
  pixelSize: Props['pixelSize'],
  centered: Props['centered'],
  fillTransparencyColor: Props['fillTransparencyColor'],
}) {
  if (pixelSize > 0) {
    for (let x = 0; x < img.width + pixelSize; x += pixelSize) {
      for (let y = 0; y < img.height + pixelSize; y += pixelSize) {
        let xColorPick = x;
        let yColorPick = y;

        if (x >= img.width) {
          xColorPick = x - (pixelSize - (img.width % pixelSize) / 2) + 1;
        }
        if (y >= img.height) {
          yColorPick = y - (pixelSize - (img.height % pixelSize) / 2) + 1;
        }

        const rgba = ctx.getImageData(xColorPick, yColorPick, 1, 1).data;
        // TODO: add support for png transparent background
        // need to create another canvas and duplicate process?
        // one canvas to get the data from
        // one to paint pixels into
        ctx.fillStyle =
          rgba[3] === 0
            ? fillTransparencyColor
            : `rgba(${rgba[0]},${rgba[1]},${rgba[2]},${rgba[3]})`;

        if (centered) {
          ctx.fillRect(
            Math.round(x - (pixelSize - (img.width % pixelSize) / 2)),
            Math.round(y - (pixelSize - (img.height % pixelSize) / 2)),
            pixelSize,
            pixelSize
          );
        } else {
          ctx.fillRect(x, y, pixelSize, pixelSize);
        }
      }
    }
  }
} 
</script>

<template>
  <canvas ref="canvas"></canvas>
</template>
