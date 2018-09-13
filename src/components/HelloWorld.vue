<template>
<div>
  <div class="menu_container" ref="menuHome" @click="toggleMenu">
    <img :src="menuSrc" alt="">
  </div>
  <div class="menu_item" v-for="(item, index) in menuItems" :key="index" :id="item.name">
    <img :src="item.src" alt="">
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      menuItems: [
        { name: 'menu1', src: require('../assets/emoji.png') },
        { name: 'menu2', src: require('../assets/cart.png') },
        { name: 'menu3', src: require('../assets/folder.png') },
        { name: 'menu4', src: require('../assets/home.png') },
        { name: 'menu5', src: require('../assets/my.png') }
      ],
      openFlag: false, // 展开合并标志
      operators: ['+', '+'] // 用户记录展开动画的xy方向 正的还是负的
    }
  },
  props: {
    menuSrc: {
      default: require('../assets/menu.png')
    },
    // menuItems: {
    //   type: Array
    // },
    position: {
      default: 'LT' // 可选择LT、LB、RT、RB4个角落
    },
    width: {
      default: 50
    },
    baseDistance: {
      default: 150
    },
    menuBg: {
      default: 'white'
    },
    itemBg: {
      default: 'white'
    }
  },
  mounted() {
    this.$refs.menuHome.style.width = this.width + 'px'
    this.$refs.menuHome.style.height = this.width + 'px'
    this.$refs.menuHome.style.lineHeight = this.width + 'px'
    this.$refs.menuHome.style.background = this.menuBg
    this.menuItems.forEach(item => {
      let el = document.getElementById(item.name)
      el.style.width = `${this.width * 0.8}px`
      el.style.height = `${this.width * 0.8}px`
      el.style.lineHeight = `${this.width * 0.8}px`
      el.style.background = this.itemBg
    })
    switch (this.position) {
      case 'LT':
        this.$refs.menuHome.style.left = '20px'
        this.$refs.menuHome.style.top = '20px'
        this.menuItems.forEach(item => {
          let el = document.getElementById(item.name)
          el.style.left = '26px'
          el.style.top = '26px'
        })
        this.operators = ['+', '+']
        break
      case 'RT':
        this.$refs.menuHome.style.right = '20px'
        this.$refs.menuHome.style.top = '20px'
        this.menuItems.forEach(item => {
          let el = document.getElementById(item.name)
          el.style.right = '26px'
          el.style.top = '26px'
        })
        this.operators = ['-', '+']
        break
      case 'LB':
        this.$refs.menuHome.style.left = '20px'
        this.$refs.menuHome.style.bottom = '20px'
        this.menuItems.forEach(item => {
          let el = document.getElementById(item.name)
          el.style.left = '26px'
          el.style.bottom = '26px'
        })
        this.operators = ['+', '-']
        break
      case 'RB':
        this.$refs.menuHome.style.right = '20px'
        this.$refs.menuHome.style.bottom = '20px'
        this.menuItems.forEach(item => {
          let el = document.getElementById(item.name)
          el.style.right = '26px'
          el.style.bottom = '26px'
        })
        this.operators = ['-', '-']
        break
      default:
        this.$refs.menuHome.style.left = '20px'
        this.$refs.menuHome.style.top = '20px'
        this.menuItems.forEach(item => {
          let el = document.getElementById(item.name)
          el.style.left = '26px'
          el.style.top = '26px'
        })
        this.operators = ['+', '+']
        break
    }
  },
  methods: {
    toggleMenu() {
      if (!this.openFlag) {
        // 点击展开操作
        this.menuItems.forEach((item, index) => {
          this.toggleMenuTransition(item.name, index, false)
        })
        this.$refs.menuHome.style.transform = 'rotate(360deg)'
      } else {
        this.menuItems.forEach((item, index) => {
          this.toggleMenuTransition(item.name, index, true)
        })
        this.$refs.menuHome.style.transform = 'rotate(0)'
      }
      this.openFlag = !this.openFlag
    },
    toggleMenuTransition(name, index, revert) {
      let oneArea = 90 / (this.menuItems.length - 1) // 每一块所占的角度
      let axisX = Math.sin(
        (this.menuItems.length - 1 - index) * oneArea * 2 * Math.PI / 360 
      ) //横坐标所偏移的比例   oneArea * 2 * Math.PI / 360 表示这个角度的弧度    eg:30度角的弧度 = 2*Math.PI/360 * 30
      let axisY = Math.cos(
        (this.menuItems.length - 1 - index) * oneArea * 2 * Math.PI / 360
      ) //纵坐标所便宜的比例
      let el = document.getElementById(name)
      if (!revert) {
        setTimeout(() => {
          el.style.transitionDuration = '200ms'
          el.style.transform = `translate`
          el.style.transform = `translate(${this.operators[0]}${this
            .baseDistance * axisX}px,${this.operators[1]}${this.baseDistance *
            axisY}px)` //进行动画
        }, index * 100)
      } else {
        el.style.transitionDuration = '200ms'
        el.style.transform = `translate(0,0)`
      }
    }
  }
}
</script>

<style>
.menu_container {
  position: absolute;
  z-index: 100;
  border-radius: 50%;
  transition-duration: 400ms;
  text-align: center;
  border: 3px solid #efefef;
  box-shadow: aliceblue 1px 1px 1px;
}
.menu_item {
  position: absolute;
  border-radius: 50%;
  z-index: 99;
  border: 3px solid #efefef;
  text-align: center;
  box-shadow: aliceblue 1px 1px 1px;
}
img {
  width: 50%;
  transform: translate(-5%, 20%);
}
</style>
