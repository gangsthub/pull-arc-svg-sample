<template>
  <div ref="container" @drag="coords" @dragend="onDragEnd" draggable="false">
    <svg
      preserveAspectRatio="xMaxYMin meet"
      viewBox="0 0 100 200"
      class="svg"
      ref="svg"
      xmlns="http://www.w3.org/2000/svg"
    >
      <g>
        <rect width="100" :height="barHeight" :fill="color" />
        <path
          d="`



          M



          0 ${barHeight - 1}



          Q300,300



          100 ${barHeight - 1}



          Z`



          "
          :fill="'#FF0000'"
        />
      </g>
    </svg>
  </div>
</template>

<script>
import { tween } from 'popmotion'

export default {
  props: {
    color: {
      type: String,
      default: () => '#2bc7f7fa'
    }
  },
  data: () => ({
    barHeight: 10,
    arc: {
      rx: 10,
      ry: 0,
      xAxisRotation: 1
    },
    clientWidth: 0,
    clientHeight: 0,
    animationDuration: 500
  }),
  methods: {
    coords($event) {
      $event.preventDefault()
      const safeValues =
        $event.offsetX &&
        $event.offsetY &&
        $event.offsetX > 0 &&
        $event.offsetY > 0
      if (safeValues) {
        this.modifyBezier($event.offsetX, $event.offsetY)
      }
      requestAnimationFrame(this.coords)
    },
    getParentBox() {
      this.clientWidth = this.$refs.container.clientWidth
      this.clientHeight = this.$refs.container.clientHeight
    },
    modifyBezier(x, y) {
      const newRY = 10 * (y / this.clientHeight)
      this.arc.ry = newRY < 5 ? newRY : this.arc.ry
      this.arc.rx = 10 * (x / this.clientWidth)
      this.arc.xAxisRotation = Math.abs(Math.cos(x / y))
    },
    onDragEnd() {
      tween({
        from: [this.arc.rx, this.arc.ry],
        to: [10, 0]
      }).start(val => {
        ;[this.arc.rx, this.arc.ry] = val
      })
    }
  },
  mounted() {
    if (window && window.document) {
      requestAnimationFrame(this.coords)
      this.getParentBox()
      window.addEventListener('resize', () => {
        this.getParentBox()
      })
    }
  }
}
</script>

<style scoped>
.container {
  position: relative;
  z-index: 0;
}
.svg {
  width: 100%;
  -webkit-user-drag: none;
  user-drag: none;
}
</style>
