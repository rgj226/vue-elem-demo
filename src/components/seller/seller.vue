<template>
  <div class="sellerscroll" ref="seller">
  	<div class="seller-wrap">
  	  <div class="overview">
        <div class="overcounent">
          <h1>{{seller.name}}</h1>
          <div>
            <star class="inline" :size="36" :score="seller.score"></star>
            <span class="yuecount">月售{{seller.sellCount}}单</span>
          </div>
        </div>                
      </div>
      <div class="notice">
        <ul>
          <li>
            <p class="title">起送价</p>
            <p class="jia">{{seller.minPrice}}<span>元</span></p>
          </li>
          <li>
            <p class="title">商家配送</p>
            <p class="jia">{{seller.deliveryPrice}}<span>元</span></p>
          </li>
           <li>
            <p class="title">平均配送时间</p>
            <p class="jia">{{seller.deliveryTime}}<span>分钟</span></p>
          </li>
        </ul>
      </div>
      <div class="line"></div>
      <!-- 公告活动-->
     <div class="activitieswrp">
       <div class="activities">
         <h1>活动与公告</h1>
         <p class="txt">{{seller.bulletin}}</p>  
          <ul v-if="seller.supports" class="supports-warp">
            <li class="supportsitem" v-for="(item,index) in seller.supports">
              <span class="icon" :class="classMap[seller.supports[index].type]"></span>
              <span class="text">{{seller.supports[index].description}}</span>
            </li>
          </ul>
       </div>
     </div>
     <div class="line"></div>
     <!--商家实景-->
     <div class="sjwrap">
       <h1>商家实景</h1>
       <div class="imgwrap">
         <div class="img-box">
           <div class="sjimg" v-for="item in seller.pics"><img :src="item"/></div>
         </div>
       </div>
     </div>
  	</div>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from '../star/star'
  import BScroll from "better-scroll"; 
  export default {
    props:{
      seller:{
        type:Object
      }
    },
    components:{
      star
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    watch:{  //响应数据变化，seller在$nextTick时候是没有数据的，在数据变化时候添加滚动时间，但是这里的高度计算有问题。在计算高度的时候html没有渲染完毕，导致高度不够
      seller:function(){
        console.log(1)  
        this.installscroll()
      }
    },
    mounted: function () {
      this.$nextTick(function () {
        //console.log(this.seller)
        this.installscroll() 
      })
    },
    methods:{
      installscroll(){       
        if(!this.scroll){
          this.scroll= new BScroll(this.$refs.seller,{
            click:true
          })
        }
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  .sellerscroll{
    overflow: hidden;
    position: absolute;
    left: 0;
    top: 4.68rem;
    bottom: 0;
    width: 100%;
    .seller-wrap{     
      .overview{
        padding: 36px 36px 0 36px;
        .overcounent{
          border-bottom: 1px solid rgba(7,17,27,0.3);
          padding-bottom: 36px;
        }
        h1{
          font-size: 14px;
          color: rgb(7,17,27);
          line-height: 28px;
          margin-bottom: 16px;
        }
        .inline{
          display: inline-block;
          margin-right: 16px;
        }
        .yuecount{
          font-size: 10px;
          color: rgb(77,85,93);
          line-height: 36px;
        }
      }
      .notice{
        padding: 36px;
        ul{
          display: flex;
          li{
            flex: 1;
            text-align: center;
            border-right: 1px solid rgba(7,17,27,0.3);
           &:last-child{
              border: none;
            }
            .title{
              font-size: 10px;
              color: rgb(147,153,159);
              line-height: 20px;
              margin-bottom: 8px;
            }
            .jia{
              font-size: 24px;
              font-weight: 200;
              line-height: 48px;
              color: rgb(7,17,27);
              span{
                font-size: 10px;
              }
            }
          }
        }       
      }
      .line{
        width: 100%;
        height: 0.42667rem;
        border-top: 1px solid rgba(7, 17, 27, 0.1);
        border-bottom: 1px solid rgba(7, 17, 27, 0.1);
        background: #f3f5f7;
      }
      .activitieswrp{
        padding: 36px 36px 0 36px;
        .activities{
          h1{
            font-size: 14px;
            color: rgb(7,17,27);
            line-height: 28px;
            margin-bottom: 16px;
          }
          .txt{
            font-size: 12px;
            font-weight: 200;
            color: rgb(240,20,20);
            line-height: 48px;
            margin-bottom: 32px;
          }
          .supportsitem{
            font-size: 0px;
            font-weight: 200;
            line-height: 32px;
            color: rgb(7,17,27);
            padding: 32px 0;
            border-top:1px solid rgba(7,17,27,0.3);
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
              color: rgb(7,17,27);
            }
          }
        }
      }
      .sjwrap{
        padding: 36px 0 36px 36px;
        h1{
          font-size: 14px;
          color: rgb(7,17,27);
          line-height: 28px;
          margin-bottom: 16px;
        }
        .imgwrap{
          width: 100%;
          overflow: hidden;
          white-space: nowrap;
          .img-box{
            font-size:0;
            .sjimg{
              display: inline-block;
              margin-right: 12px;
              height: 240px;
              height: 180px;
              &:last-child{
                margin:0
              }
              img{
                width: 240px;
                height: 180px;
              }
            }
          }
        }
      }
    }
  }
</style>