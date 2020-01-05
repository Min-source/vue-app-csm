<template>
    <briup-fulllayout title="常用地址">
    <van-list>
      <van-cell
      v-for="item in addresses"
      :key="item.id"
      :title="item.province+' '+item.city+' '+item.area+'  '+item.address"/>
    </van-list>
    <van-button block type="default" @click="toAddressEditHandler">添加</van-button>
    </briup-fulllayout>
</template>
<script>
import {get} from "../../../http/axios";
import {mapState} from "vuex";
export default {
    data(){
        return{
            addresses:[]
        }

    },
    created(){
        this.loadAddresses();
    },
    computed:{//计算属性
        //将状态机中的user对象中的info对象获取到
        ...mapState("user",["info"])
    },
    methods:{
        loadAddresses(){
            let id=this.info.id;
            let url="/address/findByCustomerId?id="+id;
            get(url).then((response)=>{
                this.addresses=response.data;
            })
        },
        toAddressEditHandler(){
            //编程跳转
            this.$router.push("/manager/address_edit")
        }
    }    
}
</script>
<style scoped>

</style>