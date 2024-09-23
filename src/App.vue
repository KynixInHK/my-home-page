<script setup>
import {onMounted, ref} from "vue";
import WebGLFluid from "webgl-fluid";
import axios from "axios";

const canvas = ref()

let dewvineSentence = ref("")

let show = ref(false)
let showDewvine = ref(false)

onMounted(async () => {
  show.value = true
  WebGLFluid(canvas.value, {
    SPLAT_COUNT: 50,
    BACK_COLOR: { r: 255, g: 255, b: 255 },
  })

  let dewvine = await axios.get("https://dewvine.axiomatrix.org/?length=25")
  showDewvine.value = true
  dewvineSentence.value = dewvine.data[0].sentence
})
</script>

<template>
  <canvas ref="canvas"></canvas>

  <Transition name="fade-in">
    <div class="ca" v-if="show">
      <h1>Adrian Chen</h1>
      <div class="dewvine-container">
        <Transition name="fade" mode="out-in">
          <p class="dewvine" v-if="!showDewvine">蔓露 · 加載中...</p>
          <p class="dewvine" v-else>{{ dewvineSentence }}</p>
        </Transition>
      </div>

      <div class="links">
        <a href="https://blog.kynix.tw/" class="link">造訪部落格</a>
      </div>
    </div>
  </Transition>

  <div class="copyright">
    Copyright © {{new Date().getFullYear()}} <a href="https://www.kynix.tw/" style="color: black">Adrian Chen</a>
  </div>
</template>

<style scoped>
canvas {
  width: 100%;
  height: 100%;
}

.ca {
  width: 100vw;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.dewvine {
  padding-left: 2rem;
  padding-right: 2rem;
  text-align: center;
  font-size: 1.2em;
}

.link {
  display: block;
  border: black 2px solid;
  padding: 1rem;
  color: black;
  transition: all 0.3s linear;
  margin-top: 2rem;
}

.link:hover {
  background-color: #000;
  color: #ffffff;
}

.copyright {
  position: fixed;
  bottom: 1rem;
  width: 100%;
  text-align: center;
}

.fade-in-enter-active,
.fade-in-leave-active {
  transition: all 1.5s ease;
}

.fade-in-enter-from,
.fade-in-leave-to {
  opacity: 0;
  transform: translateY(-10%);
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
