<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="tx">
        <img class="tx-img" :src="seller.avatar"/>
      </div>
      <div class="top-content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="miaoshu">
          <p>{{seller.description}}/{{seller.deliveryTime}}分钟送达</p>
        </div>
        <!--在这里不是判断不是所有的店家都有优惠信息，v-if渲染判断为为true的-->
        <div v-if="seller.supports" class="support">
          <!--优惠信息有多种，对应不同的icon，绑定一个class，去对应class数组中的class名-->
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="supports">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" v-on:click="showDetailshow" class="count">
        <span class="num">{{seller.supports.length}}个</span>
      </div>
    </div>
    <div class="notice">
      <span class="notice-title"></span>
      <span class="notice-text">{{seller.bulletin}}</span>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="" />
    </div>
  <!--  浮层-->
  <transition name="slide-fade">
    <div v-show="detailshow" class="detail">
    <!--  粘脚布局，用于关闭按钮始终在内容最底部，-->
      <div class="detail-warp">  <!--外层，要求清除浮动，最小高度为适口高度-->
        <div class="detail-main"> <!-- 内容  为底部关闭按钮预留空间，设置padding-bottom-->
          <h1 class="name">{{seller.name}}</h1>
          <div class="star-warp">
             <star :size="48" :score="seller.score"></star>
          </div> 
          <div class="title">
            <span class="line"></span>
            <span class="text">优惠信息</span>
            <span class="line"></span>
          </div>
          <ul v-if="seller.supports" class="supports-warp">
          	<li class="supportsitem" v-for="(item,index) in seller.supports">
          	  <span class="icon" :class="classMap[seller.supports[index].type]"></span>
          	  <span class="text">{{seller.supports[index].description}}</span>
          	</li>
          </ul>
          <div class="title">
            <span class="line"></span>
            <span class="text">商家公告</span>
            <span class="line"></span>
          </div>
          <div class="supports-warp">
            <p class="supports"> {{seller.bulletin}}</p>         
          </div>
        </div>
      </div>
      <div class="detail-close" @click="showclose">  <!--关闭按钮  设置position：relative ；以及宽高。设置margin-top为负值从底部向上显示  并清除浮动-->
        <span>关闭</span>
      </div>
    </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from '../star/star.vue'; 
export default {
  data(){
    return{
      detailshow:false
    }
  },
  methods:{
    showDetailshow:function(){
      this.detailshow=true
    },
    showclose:function(){
      this.detailshow=false
    }
  },
  props:{
    seller:{
      type:Object
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
  components:{
     "star":star
  }
  
}
</script>

<style lang="scss" rel="stylesheet/scss">
  .header{
    color: #fff;
    position: relative;
    background: rgba(7,17,25,0.5);
    overflow: hidden;
    .tx-img{
      width: 128px;
      height: 128px;
      border-radius: 8px;
    }
    .content-wrapper{
      padding: 48px 24px 36px 48px;      
      .tx{
        display: inline-block;
      }
      .top-content{
        display: inline-block;
        vertical-align: top;
        margin-left: 32px;
        font-size: 14px;
        .title{
          margin:4px 0 16px 0;
          .brand{
            display: inline-block;
            vertical-align: top;
            width: 60px;
            height: 32px;
            background: url(brand@2x.png) no-repeat;
            background-size: cover;
          }
          .name{
            margin-left: 12px;
            font-size: 18px;
            line-height: 36px;
            font-weight: bold;
          }
        }
         .miaoshu p{
          line-height: 24px;
          font-size: 12px;
          margin-bottom: 20px;
        }
        .support{
          .supports{
            vertical-align: top;
            font-size: 10px;
            line-height: 24px;
            font-weight: 200;
          } 
          .icon{
            display: inline-block;
            width: 24px;
            height: 24px;
            margin-right: 8px;
            background-size: 24px 24px;
            background-repeat: no-repeat;
            &.decrease{
              background: url(decrease_1@2x.png);
            }
            &.discount{
              background: url(discount_1@2x.png);
            }
            &.special{
              background: url(special_1@2x.png);
            }
            &.invoice{
              background: url(invoice_1@2x.png);
            }
            &.guarantee{
              background: url(guarantee_1@2x.png);
            }
          }
        }
      }
      .count{
        position: absolute;
        right: 24px;
        bottom:80px ;
        padding:0 16px ;
        height: 48px;
        line-height: 48px;
        border-radius: 18px;
        background: rgba(0,0,0,0.2);
        text-align: center;
        .num{
          font-size: 10px;
          font-weight: 200;
          line-height: 48px;
          
        }
      }     
    }
    .notice{
      height: 56px;
      padding: 0 24px 0 12px;
      line-height: 56px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      background: rgba(7,17,27,0.2);
      .notice-title{
        display: inline-block;
        vertical-align: top;
        width: 44px;
        height: 24px;
        background: url(bulletin@2x.png) no-repeat;
        background-size: cover;
        margin-top: 14px;
      }
      .notice-text{
        vertical-align: top;
        font-size:10px ;
        font-weight: 200;
        line-height: 56px;
        margin-left: 0 8px;
      
      }
    }
    .background{
      position: absolute;
      left: 0;
      top: 0;
      z-index: -1;
      width: 100%;
      height: 100%;
      filter: blur(10px);
      img{
        width: 100%;
        height: 100%;
      }
    }
    .slide-fade-enter-active,.slide-fade-leave-active  {
      transition: all .3s ;
    }
    .slide-fade-enter, .slide-fade-leave-active {
      opacity: 0;
    }
    .detail{
      position: fixed;
      left: 0;
      top: 0;
      z-index: 100;
      width: 100%;
      height: 100%;
      overflow:auto;
      backdrop-filter:blur(10px);
      background:rgba(7,17,27,0.8);
      z-index: 1000;
      .detail-warp{
        clear: both;
        width: 100%;
        min-height: 100%;
        .detail-main{
          padding-top:128px ;
          padding-bottom: 64px;
          .name{
            line-height: 32px;
            font-size: 16px;
            font-weight: 700;
            text-align: center;
          }
          .star-warp{
            margin-top: 32px;
            text-align: center;
          }
          .title{
            display: flex;
            width: 80%;
            margin: 56px auto 48px auto;
            .line{
              flex: 1;
              position: relative;
              top: -12px;
              border-bottom: 2px solid rgba(225,225,225,0.2);
            }
            .text{
              padding: 0 24px;
              font-weight: 700;
              font-size: 14px;
            }
            
          }
          .supports-warp{
            width: 80%;
            margin:0 auto 56px auto;
            font-size: 0;
            .supportsitem{
              font-size: 12px;
              font-weight: 200;
              line-height: 24px;
              margin-bottom: 24px;
              &:last-child{
                margin-bottom: 0;
              }
              .icon{
                display: inline-block;
                width: 32px;
                height: 32px;
                margin:0 12px 0 24px;
                background-size: 32px 32px;
                background-repeat: no-repeat;
                vertical-align: top;
                &.decrease{
                  background-image: url(decrease_1@2x.png);
                }
                &.discount{
                 background-image: url(discount_1@2x.png);
                }
                &.special{
                  background-image: url(special_1@2x.png);
                }
                &.invoice{
                  background-image: url(invoice_1@2x.png);
                }
                &.guarantee{
                  background-image: url(guarantee_1@2x.png);
                }
              }
              .text{
                font-size: 12px;
                font-weight: 200;
                line-height: 32px;
                vertical-align: top;
              }
            }
            .supports{
              font-size: 12px;
              line-height: 48px;
              font-weight: 200;
              padding: 0 24px;
            }
          }
        }
      }
      .detail-close{
        position: relative;
        margin: -64px auto 0 auto;
        clear: both;
        text-align: center;
        font-size: 18px;
      }
    }
  }
</style>