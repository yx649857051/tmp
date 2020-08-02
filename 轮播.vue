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
