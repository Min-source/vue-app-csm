<template>
    <briup-fulllayout title="修改地址">
    <div>
        <van-cell-group>
           <van-field v-model="$route.query.telephone"  placeholder="手机号" />
        </van-cell-group>
        <van-cell-group>
           <van-field v-model="$route.query.province" placeholder="省" />
        </van-cell-group>
        <van-cell-group>
           <van-field v-model="$route.query.city" placeholder="市" />
        </van-cell-group>
         <van-cell-group>
           <van-field v-model="$route.query.area" placeholder="区" />
        </van-cell-group>
        <van-cell-group>
           <van-field v-model="$route.query.address" placeholder="详细地址" />
        </van-cell-group>
        <van-button block type="primary" @click="submitHandler">保存</van-button>
        <br>
        <van-button block type="danger" @click="deleteHandler">删除</van-button>
    </div>
    </briup-fulllayout>
</template>

<script>
import {mapState} from 'vuex'
import {post,get} from '../../../http/axios'
export default {
    data(){
        return {

            
        }
    },
    computed:{
        ...mapState('user',['info'])
    },
    methods:{
        submitHandler(){    
            let url = '/address/saveOrUpdate'
            this.$route.query.customerId=this.info.id;
            post(url,this.$route.query).then((response)=>{
                
                //返回上一个页面
             this.$router.go(-1);
             this.$toast.success('修改成功')
            })
        },
        deleteHandler(){
            let url = "/address/deleteById?id="+this.$route.query.id;
            get(url).then((response)=>{
             this.$router.go(-1);
             this.$toast.success('删除成功')
            })
        }
    },
    created(){
      
    }
}
</script>
