<template>
  <div>
    <div class="container"></div>
    <div class="colors" v-if="palette && palette.colors">
      <div class="color" v-for="(n, index) in 5" :key="index" :style="{ 'background': `#${palette.colors[index]}` }">
      </div>
    </div>
    <div class="loader">Loading...</div>
  </div>
</template>

<script>
import { mapState } from 'vuex';

export default {
  name: 'home',
  components: {
  },
  data: () => {
    return {
      resultSize: 50,
      palette: null,
    }
  },
  computed: mapState([
    'palettes',
  ]),
  methods: {
    refreshPattern() {
      this.palette = this.palettes[Math.round(Math.random() * (this.resultSize - 1))];
      const cellSize = 110 * window.devicePixelRatio;
    }
  },
  mounted() {
    this.$store.dispatch('loadPalettes', { resultSize: this.resultSize }).then( () => {
      this.refreshPattern();
    });
  },
}
</script>

<style lang="scss">
.container {
  height: 100vh;
  width: 100vw;
  overflow: hidden;

  path {
    opacity: 0;
    
    @for $i from 1 through 10 {
      &:nth-child(#{$i}n + #{$i}) {
        transition: fill .5s linear $i * 0.1s, stroke .5s linear 0.3 + $i * 0.1s, opacity $i * 0.4s;
      }
    }
  }
}

.loader {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: #ff847c;
  opacity: 0.3;
  font-size: 30px;
}

.colors {
  position: absolute;
  bottom: 0;
  width: 100%;
  border-top: 1px solid #000;
}

.color {
  height: 15px;
  width: 20%;
  display: inline-block;
  float: left;
  background-color: #000;
  transition: background-color .5s;

  @for $i from 1 through 5 {
    &:nth-child(#{$i}) {
      transition-delay: $i * 0.1s;
    }
  }
}
</style>
