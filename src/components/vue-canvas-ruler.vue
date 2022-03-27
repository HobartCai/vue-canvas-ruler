<script lang="ts">
import { defineComponent } from "vue";

type Context2D = CanvasRenderingContext2D;
let $canvas: HTMLCanvasElement | undefined;
let $ctx: Context2D | null | undefined;

const CANVAS_HEIGHT = 40;

function isCloseToInteger(num: number) {
  return Math.abs(num - Math.round(num)) < 0.0000001;
}

function draw(
  basePixelPerUnit: number,
  scale: number,
  offset: number,
  sparsity: number,
  part: number
) {
  const { width: w, height: h } = $canvas;
  $ctx.clearRect(0, 0, w, h);

  // drawBorder
  $ctx.lineWidth = 1;
  $ctx.strokeStyle = "black";
  $ctx.beginPath();
  $ctx.moveTo(0, 0);
  $ctx.lineTo(w, 0);
  $ctx.lineTo(w, h);
  $ctx.lineTo(0, h);
  $ctx.lineTo(0, 0);

  // drawScale
  const pixelPerUnit = basePixelPerUnit * scale * sparsity;
  const gap = pixelPerUnit / part;
  let index = offset % gap > 0 ? gap - (offset % gap) : -offset % gap;
  $ctx.lineWidth = 1;
  $ctx.strokeStyle = "black";
  $ctx.font = `14px serif`;
  do {
    const num = ((offset + index) / pixelPerUnit) * sparsity;
    if (isCloseToInteger(num / sparsity)) {
      // draw long scale
      $ctx.moveTo(index, h * 0.85);
      $ctx.lineTo(index, 0);
      $ctx.fillText(String(Math.round(num)), index + 5.5, h * 0.85);
    } else {
      // draw short scale
      $ctx.moveTo(index, h * 0.5);
      $ctx.lineTo(index, 0);
    }
    index += gap;
  } while (index < w);
  $ctx.closePath();
  $ctx.stroke();
}

export default defineComponent({
  props: {
    pixelPerUnit: {
      type: Number,
      default: () => {
        return 1;
      },
      validator: (val: number) => {
        return val > 0;
      },
    },
    scale: {
      type: Number,
      default: () => {
        return 1;
      },
      validator: (val: number) => {
        return val > 0;
      },
    },
    offset: {
      type: Number,
      default: () => {
        return 0;
      },
    },
    sparsity: {
      type: Number,
      default: () => {
        return 100;
      },
      validator: (val: number) => {
        return val > 0;
      },
    },
    part: {
      type: Number,
      default: () => {
        return 10;
      },
      validator: (val: number) => {
        return val > 0;
      },
    },
  },

  data() {
    return {
      height: CANVAS_HEIGHT,
    };
  },

  computed: {},

  methods: {
    draw() {
      const { pixelPerUnit, scale, offset, sparsity, part } = this;
      draw(pixelPerUnit, scale, offset, sparsity, part);
    },
  },

  mounted() {
    $canvas = this.$refs.ruler as HTMLCanvasElement | undefined;
    if (!$canvas) {
      throw new Error("Get Canvas Ref Failed");
    }
    $ctx = $canvas.getContext("2d");
    if (!$ctx) {
      throw new Error("Get CanvasRenderingContext2D Failed");
    }
    const parentEle = $canvas.parentElement;
    $canvas.width = parentEle?.clientWidth ?? 300;
    this.draw();
  },

  updated() {
    this.draw();
  },
});
</script>
<template>
  <canvas ref="ruler" :height="height"></canvas>
</template>
