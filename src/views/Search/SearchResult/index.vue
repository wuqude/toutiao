<template>
    <div>
      <!-- 搜索结果页-头部导航 -->
      <div class="search-result-container">
        <!-- 点击实现后退效果 -->
        <van-nav-bar
          title="搜索结果"
          left-arrow
          @click-left="$router.go(-1)"
          fixed
        />
         <van-list
        v-model="loading"
        :finished="finished"
        finished-text="没有更多了"
        @load="onLoad"
        :immediate-check="false"
        offset="50"
      >
        <ArticleItem v-for="article in articleList" 
          :key="article.id"
          :article='article'
          :isShow='false'
          @click.native="toArticleDetail(article)"
          />
      </van-list>

      </div>   
    </div>
</template>

<script>
import {searchResultAPI} from '../../../api/index'
import ArticleItem from '../../../components/ArtileItem'
import {timeAgo} from '../../../utils/date'
export default {
  name: 'SearchResult',
  components:{
  ArticleItem
  },
  data() {
    return {
        articleList: [], // 文章数据
          loading: false, // 加载状态
      finished: false ,// 全部加载完成状态
      page:1
    }
  },
  mounted() {
    this.getSearchResult()
  },
 methods: {
   async getSearchResult(){
    const res = await searchResultAPI({
      q: this.$route.params.keywords // 拿到关键词, keywords来源于router/index.js你定义的动态路由参数名
    })
    this.articleList = res.data.data.results
     res.data.data.results.forEach(obj => {
      obj.pubdate = timeAgo(obj.pubdate)
    })
  },
  // 触底加载更多
    onLoad () {
    if (this.articleList.length > 0) {
        this.page++
        this.getSearchResultFn()
      }
    },
      async getSearchResultFn () {
      const res = await searchResultAPI({
        q: this.$route.params.keywords, // 拿到关键词, keywords来源于router/index.js你定义的动态路由参数名
        page: this.page
      })
      console.log(res)
      // 更多数据判断
      if (res.data.data.results.length === 0) {
        this.finished = true
        return
      }

      // 数据预处理
      res.data.data.results.forEach((obj) => {
        obj.pubdate = timeAgo(obj.pubdate)
      })

      // 合并数据
      this.articleList = [...this.articleList, ...res.data.data.results]
      this.loading = false
    },
    // 去页面详情功能
    toArticleDetail(article){
      console.log(1);
      this.$router.push(`/ArticleDetail?aid=${article.art_id}`)
    }

 },
}
</script>

<style lang="less" scoped>
.search-result-container {
  padding-top: 76px;
}

.van-nav-bar{
    color: #fff;
  background-color:#007bff ;

}
</style>
