// 轮播图
<template>
  <div class="swiper-container">
    <!-- 使用动态样式更换背景图片 -->
    <div :style="{backgroundImage: 'url(' + bg + ')'}"
      class="swiper-image">
    </div>

  </div>
</template>

<script>
  export default {
    name: 'Swiper',
    props: {
      slides: { // slides接收用于切换的图片数组
        type: Array,
        validator (value) { // 判断数组元素类型是否为字符串
          return value.every(item => Object.prototype.toString.call(item) === '[object String]')
        }
      },
      interval: { // 设置切换间隔时间，默认3s
        type: Number,
        default: 3
      },
      value: { // ① 自定义v-model，用于接收幻灯片页码，默认为0
        type: Number,
        default: 0
      }
    },

    data () {
      return {
        timer: null
      }
    },

    computed: {
      bg () {
        return this.slides[this.value]
      }
    },

    mounted () {
      this.$nextTick(function () {
        this.initTimer()
      })
    },

    methods: {
      initTimer () {
        this.timer = setInterval(() => { // 设置定时器，定时切换幻灯片
          // ② 自定义v-model，通知父级修改当前页码
          this.$emit('input', (this.value + 1) % this.slides.length)
        }, this.interval * 1000)
      },
      toggleIndex (index) { // 当鼠标划过幻灯片切换按钮时
        this.$emit('input', index) // ② 自定义v-model，通知父级修改当前页码
        clearInterval(this.timer) // 清楚定时器
      }
    }

  }
</script>

<style scoped>
  .swiper-container {
    position: relative;
    margin-left: 0px;
    width: 125%;
    height: 100%;
  }
  .swiper-image {
    width: 80%;
    height: 100%;
    background-size: cover;
    background-repeat: no-repeat;
  }
</style>
