<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">     
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
//组件的注册，注册的时候不能用vue的一些关键字，现在的header在components中改为v-header
// 在vue中尽量使用v-开通的
 import header from './components/header/header.vue'; 

export default {
  data(){
    return{       //resource数据请求返回的对象
      seller:{}
    };
  },
  created(){
    this.$http.get('api/seller').then(response => {
      response=response.body;      
      if(response.errno===0){
        this.seller=response.data  //seller数据絮叨传输到header组件中，在v-header中绑定seller
        console.log(this.seller)
      }
    })
  },
   components:{
     'v-header':header
   }
}
</script>

<style lang="scss" rel="stylesheet/scss">
.tab{ 
    width: 100%;
    height: 80px;
    line-height: 80px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    display: flex;
    .tab-item{
      flex: 1;
      a{
        display: block;
        text-align: center;
        font-size: 14px;
      }
      a:hover{
        text-decoration: none;
        
      }
    }
    .router-link-active{
      color: red;
    }
    
}
</style>
