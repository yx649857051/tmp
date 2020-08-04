//   "swiper": "^5.2.0",  "vue-awesome-swiper": "^4.1.1",

<template>
  <swiper class="swiper" :options="swiperOption">
    <swiper-slide v-for="banner in banners" :key="banner">
      <img @click="handleClickSlide(banner)" style="height: 100%; width: 100%; max-height: 100%; max-width: 100%" :src="banner">
    </swiper-slide>
    <div slot="pagination" class="swiper-pagination" />
    <div slot="button-prev" class="swiper-button-prev" />
    <div slot="button-next" class="swiper-button-next" />
  </swiper>
</template>

<script>
import { Swiper, SwiperSlide } from 'vue-awesome-swiper'
import 'swiper/css/swiper.css'

export default {
  name: 'SwiperExampleLoop',
  title: 'Loop mode / Infinite loop',
  components: {
    Swiper,
    SwiperSlide
  },
  data() {
    return {
      banners: [
        'https://images.pexels.com/photos/2025870/pexels-photo-2025870.jpeg?cs=srgb&dl=pexels-louis-2025870.jpg&fm=jpg',
        'https://images.pexels.com/photos/3356548/pexels-photo-3356548.jpeg?cs=srgb&dl=pexels-raphael-brasileiro-3356548.jpg&fm=jpg',
        'https://images.pexels.com/photos/4755027/pexels-photo-4755027.jpeg?cs=srgb&dl=pexels-kira-louw-4755027.jpg&fm=jpg'
      ],
      swiperOption: {
        slidesPerView: 1,
        spaceBetween: 30,
        loop: true,
        pagination: {
          el: '.swiper-pagination',
          clickable: true
        },
        // speed: 1000,
        // autoplay: {
        //   delay: 2000,
        //   disableOnInteraction: false
        // },
        navigation: {
          nextEl: '.swiper-button-next',
          prevEl: '.swiper-button-prev'
        }
      }
    }
  },
  mounted() {
    const date = [
      {
        year: '2021',
        month: ['05', '03', '02', '06', '09']
      },
      {
        year: '2020',
        month: ['02', '03', '07', '09', '10']
      }
    ]
    // 排序年
    const sortArr = date.sort((a, b) => Number(a.year) - Number(b.year))
    sortArr.forEach(v => {
      // 排序月
      v.month.sort((a, b) => Number(a) - Number(b))
      let startM // 记录开始位置
      let lastM // 记录循环上次数据
      const monthLen = v.month.length
      v.month.forEach((cv, ci) => {
        // 第一项
        if (ci === 0) {
          startM = cv
          lastM = cv
        }
        // 不连续
        if (Number(cv) - Number(lastM) > 1) {
          if (startM === lastM) {
            // 单个独立月
            console.log(`${v.year}-${startM}`)
          } else {
            // 到此，之前连续月
            console.log(`${v.year}-${startM}-${lastM}`)
          }
          startM = cv
        }
        lastM = cv
        // 最后一项
        if (ci === monthLen - 1) {
          if (startM === lastM) {
            // 单个独立月
            console.log(`${v.year}-${startM}`)
          } else {
            // 到此，之前连续月
            console.log(`${v.year}-${startM}-${lastM}`)
          }
        }
      })
    })
  },
  methods: {
    handleClickSlide(url) {
      console.log('当前点击索引：', this.banners.findIndex(v => v === url))
      window.open(url)
    }
  }
}
</script>

<style scoped>
  .swiper {
    height: 300px;
    width: 100%;
  }

  .swiper swiper-slide {
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    font-weight: bold;
    font-size: 20px;
    background-color: gray;
  }

  /deep/ .swiper-button-prev , .swiper-button-next {
    color: white;
    --swiper-navigation-size: 10px;
  }
</style>
