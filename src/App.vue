<script setup lang="ts">
  import { ref, computed } from 'vue'
  import { VueInfiniteViewer } from 'vue3-infinite-viewer';
  import VueDraggableResizable from 'vue-draggable-resizable'

  const zooms              = Array.from(new Array(40)).map((_,index) => (index+1) *5)
  const defaultZoomIndex   = zooms.findIndex(v => v === 100);     
  const zoom               = ref(defaultZoomIndex); 
  const image = ref(null);
  const scale = ref<number>(1)
  const scaleComputed = computed(() => ((scale.value - 1) * 100 + 150 ))
  const zoomFactor         = computed(() => zooms[zoom.value] / 100);

  function onScroll(e: any) : void {
    if(e.ctrlKey) e.deltaY < 0 ? zoomIn() : zoomOut();
  }
  function zoomIn()    { if(zoom.value < zooms.length - 1) zoom.value++; }
  function zoomOut()   { if(zoom.value > 0)                zoom.value--; }

  function onZoom(e : Event){
    const target  = e.target as HTMLInputElement

    scale.value = 1 + (parseInt(target.value) - 150) / 100
  }

  // https://stackoverflow.com/questions/19484707/how-can-i-make-an-svg-scale-with-its-parent-container

</script>

<template>
  <div class="product-toolbar">
    <div>
      <input type="range" min="1" max="300" :value="scaleComputed" class="slider" id="myRange" :onInput="onZoom">
    </div>
  </div>
  <VueInfiniteViewer useWheelScroll=true class="view-infinite-viewer" :zoom="zoomFactor" @scroll="onScroll" @wheel="onScroll">
    <div class="viewport">
      
      <div class="product-container">
        <!-- <img src="/claquette.svg" width="100%"/> -->

        <div class="image-wrapper"> 
          <VueDraggableResizable className="user-image" style="width:100%">
              <img ref="image" src="/iron-man.jpg" width="100%" :style="{ transform: `scale(${scale})`}"/>
          </VueDraggableResizable>
        </div>
      </div>
    </div>
      
  </VueInfiniteViewer>
</template>

<style>


.product-container {
  position: relative;
  /* width: auto;
  height: auto; */
  width: 600px;
  height: 400px;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  display: flex;
  justify-content: center;
  align-content: center;
  /* clip-path: url("/product.svg#claquette"); */
  mask-image: url(/claquette.svg);
  mask-repeat: no-repeat;
  mask-position: 50% 50%;
  /* background: red; */
}

.slider {
  width: 100%;
}

.product-toolbar {
  display: flex;
  width: 100%;
  position: absolute;
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
  background: #212121
}

.user-image {
  width: 100%;
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
