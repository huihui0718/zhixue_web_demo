<template>
  <div class="news-detail">
    <AwHeader class="news_header" ref="news_header"></AwHeader>
    <div class="container">
      <div class="left">
        <el-breadcrumb separator-class="el-icon-arrow-right">
          <el-breadcrumb-item :to="{ path: '/index' }">首页</el-breadcrumb-item>
          <el-breadcrumb-item>{{ newsdetail.type }}</el-breadcrumb-item>
          <el-breadcrumb-item>{{ newsdetail.title }}</el-breadcrumb-item>
        </el-breadcrumb>
        <div class="content">
          <h1>{{ newsdetail.title }}</h1>
          <span>{{ newsdetail.publish_time }}</span>
          <el-divider><i class="el-icon-view"></i></el-divider>
          <article class="article" v-html="newsdetail.content"></article>
        </div>
      </div>
      <hot-news class="right"></hot-news>
    </div>
    <!-- <AwFooter></AwFooter> -->
  </div>
</template>

<script setup>
import AwHeader from '@/components/public/Header'
import AwFooter from '@/components/public/Footer'
import HotNews from '@/components/HotNews'
import { computed, onMounted, reactive, ref } from 'vue'
import { onBeforeRouteLeave, useRoute, useRouter } from 'vue-router'
import { useStore } from 'vuex'
import { getNewsDetailsApi } from '@/apis/news'

const route = useRoute()
const router = useRouter()

const store = useStore()

const newsdetail = ref(
  {
    title: '',
    publish_time: '',
    content: '',
    type: ''
  }
)

const newspath = computed(() => route.params?.id ?? '')
debugger
console.log(newspath)
onMounted(() => {
  store.commit('setShadowActive', {
    headerLogoShow: false
  })

  store.commit('setShadowActive', {
    headerShadowActive: true
  })

  store.commit('setNavDarkActive', {
    navDarkActive: true
  })

  store.commit('setHeaderShow', {
    headerShow: false
  })
  getNewsDetails()
})

// 获取新闻详情
async function getNewsDetails () {
  const { data: res } = await getNewsDetailsApi(newspath.value)
  // this.$message.success('获取成功')
  newsdetail.value = {
    title: res.newsTitle,
    publish_time: res.publishTime,
    content: res.newsContent,
    type: '最新动态'
  }
}
</script>

<style lang = "less" scoped>
.news_header {
  background-color: rgba(255, 255, 255, .5);
  backdrop-filter: blur(10px);
  /* border-bottom: 1px solid #eff0f1; */
}

.container {
  padding-top: 60px;
  position: relative;
  display: flex;
  justify-content: space-between;
  max-width: 1200px;
  /* background: #d3dce6; */
  min-height: 580px;
  margin: 0 auto;
}

.el-breadcrumb {
  height: 40px;
  font-size: 13px;
  padding-top: 40px;
}

:deep(.el-breadcrumb__item){
  &:last-child{
    span{
      color: #f84521;
    }
  }
}

.content {
  width: 860px;

  h1 {
    font-size: 23px;
    line-height: 30px;
    padding: 20px 0 14px;
  }

  span {
    color: @regular-text-color;
    line-height: 18px;
  }
}

.article {
  margin-top: 5px;
  overflow: hidden;
}

.right {
  margin-left: 50px;
  margin-top: 80px;
}
</style>
