<template>
  <div class="canvas default w-full h-full flex flex-col justify-between items-center overflow-hidden">
    <div class="absolute flex flex-row justify-between items-center bg-white rounded-b-2xl">
      <VHeader></VHeader>
    </div>
    <canvas :style="cursor + canvasBg"
            ref="canvas"
            id="canvas"
            class="w-full h-screen"
            resize></canvas>
    <VDialogue class="absolute top-1/2 left-0 transform -translate-y-2/3 transition duration-500 ease-in-out"
               ref="sidebar"
               @click="toggleSideBar"></VDialogue>
    <!-- <VNext class="absolute bottom-12 right-12"
           @click="exportSvg"></VNext> -->
  </div>
</template>

<script>
import VHeader from '@/components/VHeader'
import VDialogue from '@/components/VDialogue'
import VNext from '@/components/VNext'
import setup from '@/utils/setup'
import { clearProject, createLayer } from '@/utils/shared'

export default {
  components: {
    VHeader,
    VDialogue,
    VNext
  },
  data() {
    return {
      isInit: false,
      vdrawArgs: {
        size: {
          width: 0,
          height: 0
        },
        zoom: 0
      },
      svg: ''
    }
  },
  methods: {
    toggleSideBar() {
      this.$refs['sidebar'].$el.classList.toggle('-translate-x-44')
    },
    exportSvg() {
      console.log('exportSvg')
      let newSvg = paper.project.exportSVG()
      let encodedData = btoa(new XMLSerializer().serializeToString(newSvg))
      let url = 'data:image/svg+xml;base64,' + encodedData
      this.$store.commit('setDrawing', url)
    }
  },
  computed: {
    cursor() {
      const tool = this.$store.state.toolName
      if (tool === null || tool === 'brush') return `cursor: auto;`
      if (tool !== 'select') return `cursor: none;`
      return `cursor: auto;`
    },
    canvasBg() {
      return `background-color: ${this.$store.state.canvasArgs.bgColor}fa;`
    },
    canvas() {
      return this.$refs.canvas
    }
  },
  mounted() {
    setup(this)
    clearProject()
    createLayer()
    this.$store.commit('setTool', 'pencil')
  },
  filters: {
    toFixed(value) {
      return value.toFixed(2)
    }
  }
}
</script>

<style>
.canvas {
  margin: 0;
  padding: 0;
  overflow: hidden;
  background: #f9f9f9;
}

.default {
  background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0naHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnIHdpZHRoPScxNScgaGVpZ2h0PScxNSc+CiAgPHJlY3Qgd2lkdGg9JzUwJyBoZWlnaHQ9JzUwJyBmaWxsPSIjRjlGOUY5IiAvPgogIDxjaXJjbGUgY3g9IjEiIGN5PSIxIiByPSIwLjgiIGZpbGw9IiNDQUNBQ0IiLz4KPC9zdmc+');
  background-repeat: repeat;
  background-position: top;
}
</style>