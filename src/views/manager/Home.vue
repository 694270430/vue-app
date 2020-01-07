<template>
  <div class="home">
    <!-- 头部图片 -->
    <header class="header">
      <img src="../../assets/home.jpg" alt="">
    </header>
   <!-- 内容区域 -->
   <div>
     <!-- 分类6个 -->
    <van-grid :column-num="3">
      <van-grid-item
        v-for="value in categories"
        :key="value.id"
        :icon="value.icon"
        :text="value.name"
      />
    </van-grid>
     <!-- 产品n个-->
    <!-- <van-grid :column-num="1" icon-size="400px">
      <van-grid-item
        v-for="value in products"
        :key="value.id"
        :icon="value.photo"
        :text="value.name"
      />
    </van-grid> -->
    <briup-product-item 
      @click="toBuyHandler(p)"
      v-for="p in products"
      :key="p.id"
      :data="p"
    >
    </briup-product-item>
   </div>
  </div>
</template>
<script>
import {get,post} from '../../http/axios'
export default {
  data(){
    return{
      categories:[],
      products:[]
    }
  },
  created(){
    //加载产品
    this.loadProducts();
    //加载栏目信息
    this.loadCategories();
  },
  methods:{
    toBuyHandler(p){
      //跳转到订单确认y界面，并携带数据p
      this.$router.push({
        path:"/manager/order_confirm",
        query:p
      })
    },
    //加载栏目信息
    loadCategories(){
      let url = "/category/findAll";
      get(url).then((response)=>{
        //push forEach slice [1,2,3,4,5,6,7]
        this.categories=response.data.slice(0,6);//slice 将查询结果的，数组中的前六个元素获取到
      })
    },
    //加载产品信息
    loadProducts(){
      let url="/product/query";
      let params={page:0,pageSize:10,}
      post(url,params).then((response)=>{
        this.products=response.data.list;
      })
    }
  }
}
</script>
<style scoped>
.home {
  padding-bottom: 50px;
}
.header {
  height: 300px;
  overflow: hidden;
}
.header img {
  width: 100%;
}
</style>