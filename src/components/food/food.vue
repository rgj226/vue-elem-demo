<template>
  <transition name="foodfade">
    <div class="foods" v-show="showfood" ref="foods">
      <div>
      <div class="fn-imgbox">
        <img :src="food.image" alt="" />
        <span class="back" @click="off"> 返回 </span>
      </div> 
      <div class="content">
        <div class="notice">
          <h1 class="titlt">{{food.name}}</h1>
          <p> <span class="mouthcount">月售{{food.sellCount}}份</span><span class="good">好评率{{food.rating}}%</span></p>
          <div class="pricecount">
            <div class="peicewrap">
              <span class="price">￥{{food.price}}</span>
              <span class="oldprice" v-show="this.food.oldPrice">￥{{food.oldPrice}}</span>
            </div>
            <div class="carnotice" v-show="this.food.count<=0 || !this.food.count" @click="countad">加入购物车</div>
            <div class="carwrap"> <cartadd :food="food"></cartadd> </div>           
          </div>
        </div>
        <div class="line"></div>
       <!-- 商品介绍-->
        <div class="introduce" v-show="this.food.info">
         <h1 class="title">商品介绍</h1>
         <p class="introducetxt">{{food.info}}</p>
        </div>
        <div class="line" v-show="this.food.info"></div>
       <!-- 评价-->
       <div class="rating" v-show="this.food.ratings">
         <h1 class="title">商品评价</h1>
         <rating v-on:ratingtype='showtype' :desc="desc" :selecttype="selecttype" :onlycount="onlycount" :ratings="food.ratings"></rating>
         <div class="ratinglist">
           <ul v-show="food.ratings">
             <li v-show="needshow(rating.rateType,rating.text)" v-for="rating in food.ratings" class="rating-item">
               <div class="user-warp">
                 <span class="time">{{rating.rateTime | formatDate}}</span>
                 <span class="user">{{rating.username}}</span>
               </div>
               <p class="text"><span class="zan" :class="{'active':rating.rateType==0,'off':rating.rateType==1}"></span> {{rating.text}}</p>
             </li>
           </ul>
           <div class="notice" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
         </div>
       </div>
      </div>
      </div>
    </div>
  </transition>
  
</template>

<script type="text/ecmascript-6">
  import vue from 'vue';
  import cartadd from "../cartadd/cartadd";  
  import BScroll from "better-scroll";
  import rating from "../rating/rating";
  import {formatDate} from "../../common/js/date";  //用花括号引入的是一个变量，通过逗号链接可以引入多个变量
  
  const positive = 0;   //评价选择的选择的三种状态。正常，不推荐/不满意，所有
  const negative = 1;
  const all = 2;
  export default {
    data(){
      return{
        showfood:false,
        selecttype:all,
        onlycount:true,
        desc:{
          all:'全部',
          positive:'推荐',
          negative:'吐槽'
        }
      }
    },
    props:{
      food:{
        type:Object
      }
    },
    methods:{
      show(){        
        this.showfood=true;
        this.selecttype=all,
        this.onlycount=false      
        this.$nextTick(() => {
          if (!this.scroll) {   //避免多次触发。同页面多次更新数据，会出现click多次触发
            this.scroll = new BScroll(this.$refs.foods, {   
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      },
      off(){
        this.showfood=false
      },
      countad(){
        vue.set(this.food,'count',1)
      },
      showtype(type){        
        this.selecttype=type;
        this.$nextTick(()=>{  //勾选选项后异步更新dom高度
          this.scroll.refresh();
        })       
      },
      onlynum(data){
        this.onlycount= !data;
        this.$nextTick(()=>{  //勾选选项后异步更新dom高度
          this.scroll.refresh();
        })
      },
      needshow(type,text){
        if(this.onlycount && !text){
          return false
        }
        if(this.selecttype===all){
          return true
        }else{
          return type===this.selecttype
        }
      }
    },
    created() {
      this.$root.eventHub.$on('onlynum',(data) => {
        this.onlynum(data)
      });
    },
    filters:{  //时间戳转换为时间
      formatDate(time){
        let date=new Date(time);
        return formatDate(date,'yyyy-MM-dd hh:mm')  //通用时间转换，单独js文件
      }
    },
    components:{
      cartadd,
      rating
    }
  };
</script>

<style lang="scss" rel="stylesheet/scss">
  .foodfade-enter-active ,.foodfade-leave-active{
    transition: all .2s linear;
  }
  .foodfade-enter, .foodfade-leave-active {
    transform: translateX(100%);
    opacity: 0;
  }
  .foods{
    position: fixed;
    left: 0;
    top: 0;
    bottom: 103px;
    z-index: 50;
    background: #fff;
    width: 100%;
    overflow: hidden;
    .fn-imgbox{
      position: relative;
      width: 100%;
      height: 0;
      padding-top: 100%;
      img{
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
      }
      .back{
        position: absolute;
        display: inline-block;
        height: 35px;
        line-height: 35px;
        background: #ccc;
        left: 40px;
        top: 40px;
      }
    }
    .content{
      .notice{
        padding: 36px;
        .titlt{
          font-size: 14px;
          font-weight: 700;
          color: rgb(7,17,27);
          line-height: 28px;
          margin-bottom: 16px;
        }
        .mouthcount,.good{
          font-size: 10px;
          color: rgb(147,153,159);
          line-height: 20px;
        }
        .good{
          margin-left: 24px;
        }
      }
      .pricecount{
        padding-top: 36px;
        position: relative;
        .peicewrap{
          .price{
            font-size: 14px;
            font-weight: 700;
            color: rgb(240,20,20);
            line-height: 48px;
          }
          .oldprice{
            font-size: 10px;
            color: rgb(147,153,159);
            line-height: 48px;
            margin-right: 24px;
            text-decoration: line-through;
          }
        }
        .carnotice{
          position: absolute;
          right: 0;
          top: 36px;
          font-size: 12px;
          color: #fff;
          line-height: 24px;
          height: 48px;
          padding: 0 24px;
          line-height: 48px;
          background: rgb(0,160,220);
          border-radius: 24px;
          z-index: 100;
        }
        .carwrap{
          position: absolute;
          right: 0;
          top: 36px;
        }
      }
      .introduce{
        padding: 0 36px;
        .title{
          margin: 36px 0 12px 0;
          font-size: 14px;
          font-weight: 700;
          color: rgb(7,17,27);
          line-height: 28px;
          margin-bottom: 16px;
        }
        .introducetxt{
          font-size: 12px;
          font-weight: 200;
          line-height: 48px;
          color: rgb(77,85,93);
        }
      }
      .line{
        width: 100%;
        height: 32px;
        border-top: 1px solid rgba(7,17,27,0.1);
        border-bottom: 1px solid rgba(7,17,27,0.1);
        background: #f3f5f7;
      }
      .rating{    
        padding-bottom: 35px;   
        .title{
          margin: 36px 0 12px 0;
          padding: 0 36px;
          font-size: 14px;
          font-weight: 700;
          color: rgb(7,17,27);
          line-height: 28px;
          margin-bottom: 16px;
        }
        .ratinglist{
          padding: 0 36px;
          .rating-item{
            width: 100%;
            padding: 32px 0;
            border-bottom: 1px solid rgba(7,17,27,0.1);
            .user-warp{
              overflow: hidden;
              .time{
                float: left;
                font-size: 10px;
                color: rgb(147,153,159);
                line-height: 24px;
              }
              .user{
                float: right;
                font-size: 10px;
                color: rgb(147,153,159);
                line-height: 24px;
                img{
                  width: 24px;
                  height: 24px;
                }
              }
            }
            .text{
              font-size: 12px;
              color: rgb(7,17,27);
              line-height: 32px;
              vertical-align: top;
              margin-top: 12px;
              .zan{
                display: inline-block;
                width: 24px;
                height: 24px;
                &.active{
                  background: url(height.png) no-repeat;
                  background-size: cover;
                }
                &.off{
                  background: url(off.png) no-repeat;
                  background-size: cover;
                }
              }
            }
          }
          .notice{
             font-size: 12px;
              color: rgb(7,17,27);
              line-height: 32px;
          }
        }
      }
    }
  }
</style>