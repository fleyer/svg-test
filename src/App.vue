<script setup lang="ts">
  import { ref, computed } from 'vue'
  import { VueInfiniteViewer } from 'vue3-infinite-viewer';
  import VueDraggableResizable from 'vue-draggable-resizable'

  const zooms              = Array.from(new Array(40)).map((_,index) => (index+1) *5)
  const defaultZoomIndex   = zooms.findIndex(v => v === 100);     
  const zoom               = ref(defaultZoomIndex); 
  const image = ref(null);
  const scale = ref<number>(150)
  const scaleComputed = computed(() => Math.round((1 + ((scale.value - 150) / 150))* 100) / 100)
  const zoomFactor         = computed(() => zooms[zoom.value] / 100);

  function onScroll(e: any) : void {
    if(e.ctrlKey) e.deltaY < 0 ? zoomIn() : zoomOut();
  }
  function zoomIn()    { if(zoom.value < zooms.length - 1) zoom.value++; }
  function zoomOut()   { if(zoom.value > 0)                zoom.value--; }

  function onZoom(e : Event){
    const target  = e.target as HTMLInputElement

    // scale.value = Math.round((1 + (parseInt(target.value) - 150) / 150)*100) / 100
    scale.value = parseInt(target.value)
  }
</script>

<template>
  <div class="container">
    <div class="product-toolbar">
        <!-- <input type="number" step="0.1" :value="scaleComputed" disabled :style="{ width: '50px'}"> -->
        <input type="range" min="1" max="300" :value="scale" class="slider" id="myRange" :onInput="onZoom">
        <div>{{scaleComputed}}</div>
    </div>
    <VueInfiniteViewer useWheelScroll=true class="view-infinite-viewer" :zoom="zoomFactor" @scroll="onScroll" @wheel="onScroll">
      <div class="viewport">
        
        <div class="product-container">

          <div class="image-wrapper"> 
            <VueDraggableResizable className="user-image" style="width:100%">
                <img ref="image" src="/iron-man.jpg" width="100%" :style="{ transform: `scale(${scaleComputed})`}"/>
            </VueDraggableResizable>
          </div>
          <img class="image-footer" src="/handle.svg" width="100%"/>
        </div>
      </div>
        
    </VueInfiniteViewer>
  </div>
</template>

<style>

.view-infinite-viewer {
  flex:1;
}

.container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}

.image-footer {
  position: absolute;
  top: 100%;
  right: 14px;
  padding-top: 2px;
}

.product-container {
  position: relative;
  width: 600px;
  height: 400px;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  display: flex;
  justify-content: center;
  align-content: center;
}

.product-toolbar {
  display: flex;
  align-items: center;
  width: 100%;
  margin: 0;
  padding: 10px;
  z-index: 1000;
  background: #212121;
  border-bottom: 1px solid #424242;
}

.product-toolbar div {
  
}

.product-toolbar div input {
  margin: 0;
  padding: 0;
}

.box {
  height: 300px;
  width: 400px;
  border: 1px solid #212121;
  background: #fff;
  color: #212121;
  border-radius: 4px;
  padding: 10px;
}

.image-wrapper{
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: #212121;
  mask-image: url(/claquette.svg);
  mask-repeat: no-repeat;
  mask-position: 50% 50%;
}

.user-image {
  width: 100%;
  cursor: pointer;;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.view-infinite-viewer {
  box-sizing: border-box;
  position: absolute;
  user-select: none;
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  overscroll-behavior: none;
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;
}

.viewport {
  box-sizing: border-box;
  position: relative;
  width: 100%;
  height: 100%;
}
</style>
