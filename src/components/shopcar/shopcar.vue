<template>
  <div class="shopcar">
    <div class="content" @click="listshow">
      <div class="content-left">
        <div class="logo-wrap">
          <div class="logo" :class="{'heightlight':totalcount>0}">
            <span class="icon-shopping_cart"></span>
          </div>
          <span class="count" v-show="totalcount>0">{{totalcount}}</span>
        </div><!--有商品加入的时候购物车和价格高亮-->
        <div class="price" :class="{'heightlight':totalcount>0}">￥{{totalprice}}</div>
        <div class="desc">另需配送费￥{{psprice}}元</div>
      </div>
      <div class="content-right" :class="{'payheight':this.totalprice>=this.minPrice}" @click.stop="gopay">
        <div class="pay">{{pay}}</div>
      </div>
    </div>  
    <div class="shap-list" v-show="fold">
    <div class="list-head">
      <span class="title">购物车</span>
      <span class="empty" @click="clear">清空</span>
    </div>
    <div class="list-content" ref="list">
      <ul>
        <li class="food" v-for="food in selectfood">
          <div class="name-count">
            <span class="name">{{food.name}}</span>
            <span class="pri">￥{{food.price*food.count}}</span>
          </div>        
         <div class="cartcon">
           <cartadd :food="food"></cartadd>
         </div>
        </li>
      </ul>
    </div>
  </div>   
  <div class="list-wrp" v-show="fold"  @click="listnone"></div>
  </div>  
</template>

<script type="text/ecmascript-6">
  import BScroll from "better-scroll";
  import cartadd from "../cartadd/cartadd";
  export default {
    data(){
      return{
        fold:false
      }
    },
    props:{
     psprice:{
       type:Number
     },
     minPrice:{
       type:Number
     },
     selectfood:{   //保存选择商品数量，价格的数组.从goods中获取
       type:Array,
       default(){
         return [
          {
            price:0,
            count:0  //添加的自定义属性
          }
         ];
       }
     }    
   },
   computed:{
     totalprice(){   //计算价格总和。selectfood中的数量价格相乘.
       let total=0;
       this.selectfood.forEach((food)=>{
         total+=food.price * food.count;
       });
       return total
     },
     totalcount(){    //商品数量总和
       let count=0;
       this.selectfood.forEach((food)=>{
         count+=food.count;
       });
       return count
     },
     pay(){
       if(this.totalprice===0){
         return  this.minPrice+"元起送"
         //es6写法,免拼接
         //`${this.minPrice}元起送
       }else if(this.totalprice<this.minPrice){
         let now=this.minPrice-this.totalprice  //配送起差价
         return  "还差"+now+"元起送"
       }else{
         return  "去结算"
       }
     }
   },
    methods:{
      listshow(){
        if(!this.totalcount){
          return
        }
        this.fold=!this.fold
        this.$nextTick(() => {
          if (!this.scroll) {   //避免多次触发。同组件多次更新数据，会出现click多次触发
            this.scroll = new BScroll(this.$refs.list, {   
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      },
      listnone(){
        this.fold=false
      },
      clear(){
        this.selectfood.forEach((food)=>{
          food.count=0
        });
        this.fold=false
      },
      gopay(){
        if(this.totalprice<this.minPrice){
          return
        }else{
          alert("支付"+this.totalprice+"元")
        }
      }
    },
   components:{
     cartadd
   }
  };
</script>

<style lang="scss" rel="stylesheet/scss">
  .shopcar{
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    z-index: 100;
    .content{
      display: flex;
      background:#141d27;
      .content-left{
         flex: 1;
         font-size: 0;
         .logo-wrap{
           display: inline-block;
           position: relative;
           top:-20px ;
           margin:0 24px;
           padding: 10px;
           width: 84px;
           height: 84px;
           border-radius: 50%;
           vertical-align: top;
           background: #141d27;
           z-index: 800;
           .logo{
             width: 100%;
             height: 100%;
             background:#2b343c;
             border-radius: 50%;
             background-image: url(car@2x.png);
             background-repeat: no-repeat;
             background-position: center;
             background-size: 42px 42px;
             &.heightlight{
               background: #00a0dc;
                background-image: url(heightlight.png);
                background-repeat: no-repeat;
                background-position: center;
                background-size: 42px 42px;
             }
           }
           .count{
             display: inline-block;
             position: absolute;
             top: 0;
             right: 0;
             width: 48px;
             font-size: 9px;
             height: 32px;
             line-height: 32px;
             font-weight: 700;
             color: #fff;
             background: rgb(240,20,20);
             box-shadow:0 4px 8px 0 rgba(0,0,0,0.4);
             border-radius: 28px;
             text-align: center;
           }
         }
         .price{
           display: inline-block;
           vertical-align: top;
           font-size: 18px;
           font-weight: 700;
           color: rgba(255,255,255,0.4);
           line-height: 48px;
           margin-top: 28px;
           padding-right: 12px;
           border-right: 1px solid rgba(255,255,255,0.4);
           &.heightlight{
             color: #fff;
           }
         }
         .desc{
           display: inline-block;
           vertical-align: top;
           line-height: 48px;
           margin-left: 24px;
           font-size: 14px;
           margin-top: 28px;
           color: rgba(255,255,255,0.4);
         }
      }       
      .content-right{
         flex:0 1 210px;
         width: 210px;
         background: #2b333b;
          &.payheight{
             background: #00b43c; 
             .pay{
               color: rgb(255,255,255);
               font-weight: 700;
             }
           } 
         .pay{
           font-size: 14px;
           height: 48px;
           line-height: 48px;
           margin-top: 28px;
           text-align: center;
           color: rgba(255,255,255,0.4);                 
         }
      }      
    }
    .list-wrp{
      position: fixed;
      width: 100%;
      height: 100%;
      bottom: 103px;
      left: 0;
      background: rgba(0,0,0,0.3);
      backdrop-filter: blur(0.13333rem);
    }
    .shap-list{
      position: fixed;
      width: 100%;
      bottom: 103px;
      background: #f3f5f7;
      z-index: 500;
      .list-head{
        height: 80px;
        line-height: 80px;
        padding: 0 36px;
        .title{
          float: left;
          font-size: 14px;
          font-weight: 200;
          color: rgb(7,17,27);       
        }
         .empty{
          font-size: 12px;
          color: rgb(0,160,200);
          float:right
        }  
      }
      .list-content{
        padding: 0 36px;
        max-height: 430px;
        background: #fff;
        padding-bottom: 45px;
        overflow: hidden;
        .food{
          height: 96px;
          position: relative;
          border-bottom: 1px solid rgba(7,17,27,0.1);          
          .name-count{
            width: 510px;
            height: 100%;
            .name,.pri{
              font-size: 14px;
              color: rgb(7,17,27);
              line-height: 96px;
            }
            .pri{
              float:right
            }
          }
          .cartcon{
            position: absolute;
            right: 0px;
            top: 24px;
          }
        }
      }
    }
  }
</style>