<template>
  <div class="app-container">
    <Header></Header>
    <Goods v-for="item in list" :key="item.id" :item="item"></Goods>
    <Footer :list="list" :fullState="fullState" :checkLength="checkLength" :allPrice="allPrice"></Footer>
  </div>
</template>

<script>
import Header from '@/components/Header/Header'
import Goods from '@/components/Goods/Goods'
import Footer from '@/components/Footer/Footer'
import axios from 'axios'
export default {
  components: {
    Header,
    Goods,
    Footer
  },
  data () {
    return {
      list: [],
      isAllCheck: false
    }
  },
  created () {
    this.initList()
  },
  computed: {
    fullState () {
      return this.list.every(item => item.goods_state)
    },
    checkLength () {
      return this.list.reduce((amt, item) => {
        if (item.goods_state) {
          amt += 1
        }
        return amt
      }, 0)
    },
    allPrice () {
      return this.list.filter(item => item.goods_state).reduce((amt, item) => amt += item.goods_count * item.goods_price, 0)
    }
  },
  methods: {
    async initList () {
      const { data: res} = await axios.get('https://www.escook.cn/api/cart')
      this.list = res.list
    }
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
