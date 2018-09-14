<template>
  <div class="blobs">
    <div class="circle main" @click="mainClick">+</div>
    <div class="circle sub first"  :class="{'show' : isShow}">1</div>
    <div class="circle sub second" :class="{'show' : isShow}">2</div>
    <div class="circle sub third"  :class="{'show' : isShow}">3</div>
  <!--  -->
    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
      <defs>
        <filter id="goo">
          <feGaussianBlur in="SourceGraphic" stdDeviation="5" result="blur" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -7" result="goo" />
          <feBlend in="SourceGraphic" in2="goo" />
        </filter>
      </defs>
    </svg>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShow: false
    }
  },
  methods: {
    mainClick() {
      this.isShow = !this.isShow
    }
  },
  components: {}
}
</script>

<style scoped>
.blobs {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  filter: url(#goo);
}

.circle {
  position: absolute;
  width: 90px;
  height: 90px;
  line-height: 90px;
  background: hsl(337, 70%, 58%);
  /* border-radius: 50%;
   */
  clip-path: circle(42px at center);
  transform: translate(0, -48px);
}
.circle.main {
  z-index: 2;
}
.first {
  transition: transform 0.5s ease-out 100ms;
  background: hsl(307, 70%, 58%);
}
.second {
  transition: transform 0.5s ease-out 300ms;
  background: hsl(277, 70%, 58%);
}
.third {
  transition: transform 0.5s ease-out 500ms;
  background: hsl(247, 70%, 58%);
}
.first.show {
  transform: translate(-100px, -120px);
}
.second.show {
  transform: translate(0, -150px);
}
.third.show {
  transform: translate(100px, -120px);
}
</style>
