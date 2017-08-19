<template>
  <div class="container">
    <nav class="nav-header">
      <header>
        <a href="/" class="brand">移动书城</a>
        <a href="/" class="avatar"></a>
      </header>
      <div class="swipe">
        <mt-swipe :auto="5000">
          <mt-swipe-item><img src="../assets/images/1.jpg" alt=""></mt-swipe-item>
          <mt-swipe-item><img src="http://qidian.qpic.cn/qidian_common/349573/909abf2138cac49dfee57d96b96fd361/0" alt=""></mt-swipe-item>
          <mt-swipe-item><img src="http://qidian.qpic.cn/qidian_common/349573/399190e3cdf29322a68bfda385709f73/0" alt=""></mt-swipe-item>
          <mt-swipe-item><img src="http://qidian.qpic.cn/qidian_common/349573/75cc40c5b57d9411520cdcb3dddeb5cc/0" alt=""></mt-swipe-item>
          <mt-swipe-item><img src="http://qidian.qpic.cn/qidian_common/349573/8c983e7b98c545e5b954fa4c527de296/0" alt=""></mt-swipe-item>
        </mt-swipe>
      </div>
    </nav>
    <nav class="home-nav">
      <router-link
        class="guide-nav-div"
        v-for="(item,index) in type"
        :to="{path:'category',query: {type: item.num}}"
        :key="item.num"
        @click="openBookCategory(item.num)">
        <i class="icon icon-sort"></i>
        <h4 class="guide-nav-h">{{item.word}}</h4>
      </router-link>
    </nav>
    <div v-if="!loading">
      <recommend :booklist="booklist | hot" title="热门小说"></recommend>
      <recommend :booklist="booklist | top" title="排行榜"></recommend>
      <recommend :booklist="booklist | free" title="限时免费"></recommend>
      <book-list :datalist="booklist | newbook" title="新书抢鲜"></book-list>
      <book-list :datalist="booklist | endbook" title="畅销完本"></book-list>
      <book-list :datalist="booklist | like" title="猜你喜欢"></book-list>
    </div>
    <loading v-show="loading"></loading>
  </div>
</template>
<script>
import axios from 'axios'
import Recommend from './Common/Recommend.vue'
import BookList from './Common/BookList.vue'
import Loading from './Loading/Loading.vue'
export default {
    data(){
        return {
          booklist: [],
          type: [
          {
            num: 1,
            word: '玄幻'
          },
          {
            num: 2,
            word: '修真'
          },
          {
            num: 3,
            word: '都市'
          },
          {
            num: 4,
            word: '历史'
          },
          {
            num: 5,
            word: '游戏'
          }
        ],
        loading: false
        }
    },
    created() {
      this.getData()
    },
    methods:{
      getData() {
        this.loading = true
        axios.get(`${this.common.api}/booklist`).then(res => {
          this.loading = false //获取数据完成后隐藏loading
          this.booklist = res.data
        })
      }
    },
    components: {
      Recommend,
      BookList,
      Loading
    },
  filters: {
      hot(value) {
        if (!value) return ''
        var arr = []
        value.forEach((item, index) => {
          //简单更改下首页加载数据太多,可自行修改
          if (index < 20) {
            if (index % 2 == 1) {
              arr.push(item)
            }
          }
        })
        return arr
      },
      top(value) {
        if (!value) return ''
        var arr = []
        value.forEach((item, index) => {
          //简单更改下首页加载数据太多,可自行修改
          if (index < 20) {
            if (index % 2 == 0) {
              arr.push(item)
            }
          }
        })
        return arr
      },
      free(value) {
        if (!value) return ''
        var arr = []
        value.forEach((item, index) => {
          //简单更改下首页加载数据太多,可自行修改
          if (index < 20) {
            if (index % 3 == 2) {
              arr.push(item)
            }
          }
        })
        return arr
      },
      newbook(value) {
        if (!value) return ''
        var arr = []
        value.forEach((item, index) => {
          if (index % 3 == 1) {
            arr.push(item)
          }
        })
        return arr.splice(0, 3)
      },
      endbook(value) {
        if (!value) return ''
        var arr = []
        value.forEach((item, index) => {
          if (item.serialize === '完本') {
            arr.push(item)
          }
        })
        return arr
      },
      like(value) {
        if (!value) return ''
        var arr = []
        value.forEach((item, index) => {
          if (index % 4 == 2) {
            arr.push(item)
          }
        })
        return arr.splice(0, 3)
      }
    }
  }
</script>
<style lang="less">
  .container {
    background-color: #f6f7f9;
  }

  .nav-header {
    header {
      display: flex;
      height: 45px;
      padding: 0 15px;
      justify-content: space-between;
      align-items: center;
      font-size: 20px;
      text-indent: -8888px;//隐藏文字
      .brand {
        width: 100px;
        background: url(http://qidian.gtimg.com/qdm/img/logo-qdm.0.50.svg) no-repeat left;
        background-size: 100%;
      }
      .avatar {
        width: 24px;
        height: 24px;
        background: url(../assets/images/avatar.png) no-repeat center;
        background-size: 100%;
      }
    }
  }

  .home-nav {
    display: flex;
    padding: 10px 0;
    margin: 10px 0;
    background-color: #fff;
    .guide-nav-div {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      > i {
        width: 24px;
        height: 24px;
        background-image: url(../assets/images/sprite.0.50.png);
      }
      &:nth-of-type(1) {
        i {
          background-position: -63px -28px;
        }
      }
      &:nth-of-type(2) {
        i {
          background-position: 0 0;
        }
      }
      &:nth-of-type(3) {
        i {
          background-position: 0 -30px;
        }
      }
      &:nth-of-type(4) {
        i {
          background-position: 0 -60px;
        }
      }
      &:nth-of-type(5) {
        i {
          background-position: -30px -30px;
        }
      }
    }
  }

  .swipe {
    height: 170px;
    padding: 0 15px;
    img {
      width: 100%;
      height: 100%;
    }
  }
</style>

