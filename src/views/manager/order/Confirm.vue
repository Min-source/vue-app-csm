<template>
    <briup-fulllayout title="订单确认">
    服务地址：
    <van-dropdown-menu>
      <van-dropdown-item v-model="addressId" :options="options" />
    </van-dropdown-menu>
    订单详情：
    <div style="padding:0 2em">
        <p>服务名称：{{$route.query.name}}</p>
        <p>服务简介：{{$route.query.description}}</p>
        <p>服务价格：{{$route.query.price}}</p>
        <p>服务数量：1</p>
        <p>服务小计：{{$route.query.price * 1}}</p>
    </div>
    <div style="position:fixed;bottom:0;width:100%;">
    <van-button block type="primary" @click="submitHandler">提交订单</van-button>
    </div>
    </briup-fulllayout>
</template>
<script>
import {mapState} from "vuex";
import {get,post_obj_array} from "../../../http/axios";
export default {
    data(){
        return{
            addressId:0,
            addresses:[],
            orderLines:[]
        }
    },
    created(){
        this.loadAddresses();
        //初始化订单项（将购物车产品放入到购物车）
        let orderLine={
            number:1,//限购1件
            price:this.$route.query.price,
            productId:this.$route.query.id
        };
        this.orderLines.push(orderLine);
    },
    computed:{
        ...mapState("user",["info"]),
        //[{ "id": 2228, "province": "江苏省"}]
        //=>[{text:"苏州市",value:1}]
        options:function(){
            //将addresses的数据计算为一个新的数组返回
            return this.addresses.map(item=>{
                return{
                    text:item.province+" "+item.city+" "+item.area+" "+item.address,
                    value:item.id
                }
            })
        }
    },
    methods:{
        loadAddresses(){
            let id=this.info.id;
            let url="/address/findByCustomerId?id="+id;
            get(url).then((response)=>{
                //将查询到地址信息绑定到address这个变量中
                this.addresses=response.data;
                //将查询到的地址列表中第一个地址id赋值给data中addressId,,表示默认地址
                this.addressId = this.addresses[0].id;
            })
        },
        submitHandler(){
            let url ="/order/save";
            let data ={
                customerId:this.info.id,
                addressId:this.addressId,
                orderLines:this.orderLines
            }
            console.log(data);
            post_obj_array(url,data).then((response)=>{
                this.$toast("提交成功")
                this.$router.push("/manager/order")
            });
        }
    }
    
}
</script>
<style scoped>

</style>