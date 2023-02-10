<template>
  <el-container v-show="!loading">
    <el-main>
      <Hero />
      <Editor v-on:big-loading="isLoading" />
    </el-main>
  </el-container>
  <div v-show="loading" class="loader-div">
    <ul class="loading-text">
      <li class="loading-char">L</li>
      <li class="loading-char">O</li>
      <li class="loading-char">A</li>
      <li class="loading-char">D</li>
      <li class="loading-char">I</li>
      <li class="loading-char">N</li>
      <li class="loading-char">G</li>
    </ul>
    <div class="loading-desc">
      <p>want to use it better? Contact and focus me...</p>
      <p>想要更好的使用ChatGPT么？联系我……</p>
    </div>
  </div>
</template>
<script>
import Editor from './Editor.vue'
import Hero from './Hero.vue'
export default {
  name: 'MainPage',
  components: {
    Editor,
    Hero
  },
  data() {
    return {
      loading: true
    };
  },
  created() {

    this.loading = false;

  },
  methods: {
    isLoading(loadtype) {
      console.log(loadtype)
      this.loading = loadtype
    }
  }
}
</script>

<style>
.loader-div {
  padding: 0;
  margin: 0;
  background: #292929;
}

.loading-text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.loading-desc {
  position: absolute;
  top: 75%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.loading-char {
  list-style: none;
  float: left;
  font-size: 5em;
  font-weight: bold;
  font-family: Arial;
  transform: rotateY(90deg);
  transform-origin: left;
  animation: load 2.5s infinite;
}

@keyframes load {
  0% {
    opacity: 0;
    transform: rotateY(90deg) translateX(-30px);
  }

  50% {
    opacity: 1;
    transform: rotateY(0deg) translateX(30px) scale(0.4);
  }

  100% {
    opacity: 0;
    transform: rotateY(0deg);
  }
}

ul li:nth-child(1) {
  animation-delay: 0s;
}

ul li:nth-child(2) {
  animation-delay: 0.2s;
}

ul li:nth-child(3) {
  animation-delay: 0.4s;
}

ul li:nth-child(4) {
  animation-delay: 0.6s;
}

ul li:nth-child(5) {
  animation-delay: 0.8s;
}

ul li:nth-child(6) {
  animation-delay: 1s;
}

ul li:nth-child(7) {
  animation-delay: 1.2s;
}
</style>