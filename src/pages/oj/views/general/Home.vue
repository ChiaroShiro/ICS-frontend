<template>
  <Row type="flex" justify="space-around">
    <Col :span="22">
      <!-- Existing Contest Panel -->
      <panel shadow v-if="contests.length" class="contest">
        <div slot="title">
          <Button type="text" class="contest-title" @click="goContest">{{contests[index].title}}</Button>
        </div>
        <Carousel v-model="index" trigger="hover" autoplay :autoplay-speed="6000" class="contest">
          <CarouselItem v-for="(contest, index) of contests" :key="index">
            <div class="contest-content">
              <div class="contest-content-tags">
                <Button type="info" shape="circle" size="small" icon="calendar">
                  {{contest.start_time | localtime('YYYY-M-D HH:mm') }}
                </Button>
                <Button type="success" shape="circle" size="small" icon="android-time">
                  {{getDuration(contest.start_time, contest.end_time)}}
                </Button>
                <Button type="warning" shape="circle" size="small" icon="trophy">
                  {{contest.rule_type}}
                </Button>
              </div>
              <div class="contest-content-description">
                <blockquote v-html="contest.description"></blockquote>
              </div>
            </div>
          </CarouselItem>
        </Carousel>
      </panel>

      <Announcements class="announcement"></Announcements>

      <!-- 新增的日历组件 -->
      <Calendar />

      <!-- 新增的图片展示模块 -->
      <div class="image-gallery">
        <Carousel autoplay :autoplay-speed="5000">
          <CarouselItem v-for="(image, index) in images" :key="index">
            <img :src="image" alt="Image" class="gallery-image" />
          </CarouselItem>
        </Carousel>
      </div>
    </Col>
  </Row>
</template>

<script>
import Announcements from './Announcements.vue'
import api from '@oj/api'
import time from '@/utils/time'
import { CONTEST_STATUS } from '@/utils/constants'
import Calendar from './Calendar.vue' // 引入日历组件

export default {
  name: 'home',
  components: {
    Announcements,
    Calendar // 注册日历组件
  },
  data () {
    return {
      contests: [],
      index: 0,
      images: [
        require('./p1.jpg'),
        require('./p2.jpg'),
        require('./p3.jpg')
      ]
    }
  },
  mounted () {
    let params = {status: CONTEST_STATUS.NOT_START}
    api.getContestList(0, 5, params).then(res => {
      this.contests = res.data.data.results
    })
  },
  methods: {
    getDuration (startTime, endTime) {
      return time.duration(startTime, endTime)
    },
    goContest () {
      this.$router.push({
        name: 'contest-details',
        params: {contestID: this.contests[this.index].id}
      })
    }
  }
}
</script>

<style lang="less" scoped>
  .contest {
    &-title {
      font-style: italic;
      font-size: 21px;
    }
    &-content {
      padding: 0 70px 40px 70px;
      &-description {
        margin-top: 25px;
      }
    }
  }

  .announcement {
    margin-top: 20px;
  }

  .image-gallery {
    max-width: 900px; /* 设置最大宽度 */
    margin: 20px auto; /* 垂直留出空间，水平居中 */
    h2 {
      text-align: left; /* 标题居左 */
    }
    .gallery-image {
      width: 100%; /* 调整为100%以适应 Carousel */
      height: auto; /* 自动高度，保持比例 */
      border-radius: 8px; /* 圆角 */
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2); /* 阴影效果 */
    }
 }
</style>
