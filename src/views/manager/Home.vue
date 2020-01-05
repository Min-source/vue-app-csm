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
    <van-grid :column-num="1" icon-size="400px"> 
      <van-grid-item
      v-for="value in products"
      :key="value.id"
      :icon="value.photo"
      :text="value.name"/>
    </van-grid>
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