<template>
  <div class="container">
    <van-card v-for="course in courses" :key="course.id" :thumb="course.publisherHeader">
      <div class="title" slot="title">
        课程名称: {{course.courseName}}
      </div>
      <div class="desc" slot="desc">
        上课时间： {{course.courseTime}}
        <br/>
        上课地点： {{course.coursePlace}}
        <br/>
        赏金: {{course.reward ? course.reward : 0}}
      </div>
      <div slot="footer">
        <van-button size="mini">详情</van-button>
        <van-button size="mini" type="primary" plain>代课</van-button>
      </div>
    </van-card>

    <van-sku v-model="isShowCourse" :sku="course" :goods="courseTitle" close-on-click-overlay>

    </van-sku>
  </div>
</template>
<script>
export default {
  data() {
    return {
      courses: [],
      course: {},
      isShowCourse: false
    }
  },
  computed: {
    courseTitle() {
      return {
        title: this.course.courseName,
        picture: this.course.publisherHeader
      }
    }
  },
  mounted() {
    this.$http.getCourse({status: 'open'})
    .then(res => {
      console.log('获取课程列表返回',res)
      this.courses = res.data
    })
    .catch(err => {
      this.$toast.fail('获取课程列表失败', err)
    })
  },
  methods: {

  }
}
</script>
<style lang="scss" scoped>

</style>
