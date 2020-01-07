<template>
  <div class="home">
    <header class="header">
      <img src="../../assets/home.jpg" alt="">
    </header>
    <!-- 内容部分 -->
    <div>
      <!-- 分类 6个-->
    <van-grid :column-num="3">
      <van-grid-item
      v-for="value in categories"
      :key="value.id"
      :icon="value.icon"
      :text="value.name"/>
    </van-grid>
    <briup-product-item
      @click="toBuyHandler(p)"
      v-for="p in products"
      :key="p.id"
      :data="p">
      </briup-product-item>
    </div>
  </div>
</template>
<script>
import {get,post} from "../../http/axios";
export default {
  data(){
    return{
      categories:[],
      products:[]
    }
  },
  created(){
    this.loadCategories();
    this.loadProducts();
  },
  methods:{
    loadCategories(){
      //加载目录
      let url = "/category/findAll";
      get(url).then((response)=>{
        this.categories=response.data.slice(0,6);
      })
    },
    loadProducts(){
      let url= "/product/query";
      let params={ page:0,pageSize:10 }
      post(url,params).then((response)=>{
        this.products=response.data.list.slice(0,4);
      })
    },
    toBuyHandler(p){
      // alert(JSON.stringify(p));
      this.$router.push({
        path:"/manager/order_confirm",
        query:p
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