<template>
  <div class="ratingswrap" ref='ratingwp'>
    <div class="count">
      <div class="ratingscontent">
        <div class="overview-left">
          <p class="fen">{{seller.score}}</p>
          <p class="fennotice">综合评分</p>
          <p class="notice">高于周边商家{{seller.rankRate}}</p>
        </div>
        <div class="overview-right">
          <div class="starnotive"><span class="title">服务态度</span> <star class="star" :size='36' :score='seller.serviceScore'></star><span class="score">{{seller.serviceScore}}</span></div>
          <div class="starnotive"><span class="title">食物评分</span> <star class="star" :size='36' :score='seller.foodScore'></star> <span class="score">{{seller.foodScore}}</span></div>
          <div class="starnotive"><span class="title">送达时间</span> <span class="time">{{seller.deliveryTime}}分钟</span></div>
        </div>       
      </div>
      <div class="line"></div>
      <!--评价选项-->
      <div class="ratingoption">
        <rating v-on:ratingtype='ratingtype' :desc='desc' :selecttype='selecttype' :onlycount='onlycount' :ratings='ratings'></rating>
      </div>
      <!--评价列表-->
      <div class="ratingwrap">
        <ul>
          <li v-show="needshow(rating.rateType,rating.text)" class="list-item" v-for="rating in ratings">
            <div class="user-head"><img :src="rating.avatar" alt="" /></div>
            <div class="usercontent">
              <p class="name"><span class="username">{{rating.username}}</span> <span class="time">{{rating.rateTime | formatDate}}</span></p>
              <div class="starstar">
                <star class="star" :size='24' :score='seller.serviceScore'></star>
                <span class="sdtime" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
              </div>
              <p class="txt">{{rating.text}}</p>
              <div class="recommend">
                <span class="icon" :class="{'active':rating.rateType==0,'off':rating.rateType==1}"></span>
                <span class="foodz" v-for="item in rating.recommend">{{item}}</span>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from "../star/star" 
  import rating from "../rating/rating"
  import BScroll from "better-scroll";
  import {formatDate} from "../../common/js/date";  //用花括号引入的是一个变量，通过逗号链接可以引入多个变量
  
  const positive = 0;   //评价选择的选择的三种状态。正常，不推荐/不满意，所有
  const negative = 1;
  const all = 2;
  
  export default {
    data(){
      return{
        selecttype:all,
        onlycount:true,
        desc:{
          all:'全部',
          positive:'满意',
          negative:'不满意'
        },
        ratings:[]        
      }
    },
   props:{
    seller:{
      type:Object
    }
   },
   methods:{
     ratingtype(type){
       this.selecttype=type
       this.$nextTick(()=>{  //勾选选项后异步更新dom高度
          this.myscroll.refresh();
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
   created(){
    this.$http.get('api/ratings').then(response => {
      response=response.body;      
      if(response.errno===0){
        this.ratings=response.data  //seller数据絮叨传输到header组件中，在v-header中绑定seller
        console.log(this.ratings)
        this.$nextTick(()=>{
          this.myscroll= new BScroll(this.$refs.ratingwp,{
             click:true
           })
        })
      }
    }),
    this.$root.eventHub.$on('onlynum',(data)=>{
      this.onlycount=!data;
      this.$nextTick(()=>{  //勾选选项后异步更新dom高度
        this.myscroll.refresh();
      })  
    })
   },
    filters:{  //时间戳转换为时间
      formatDate(time){
        let date=new Date(time);
        return formatDate(date,'yyyy-MM-dd hh:mm')  //通用时间转换，单独js文件
      }
    },
    components:{
      star,
      rating
    }
  }
  
</script>

<style lang="scss" rel="stylesheet/scss">
  .ratingswrap{
    overflow: hidden;
    position: absolute;
    left: 0;
    top: 351px;
    bottom: 0;
    width: 100%;
    .ratingscontent{
      display: flex;
      padding:36px 0;
      .overview-left{
        flex: 0 1 275px;
        width: 275px;
        border-right: 1px solid rgba(7,17,27,0.1);
        text-align: center;
        .fen{
          font-size: 24px;
          line-height: 56px;
          color: rgb(255,153,0);
          margin-bottom: 12px;
        }
        .fennotice{
          font-size: 12px;
          color: rgb(7,17,27);
          line-height: 24px;
          margin-bottom: 16px;
        }
        .notice{
          font-size: 10px;
          color: rgb(147,153,159);
          line-height: 24px;
        }
      }
      .overview-right{
        flex: 1 ;
        padding-left: 48px;
        .starnotive{
          margin-bottom: 16px;
          font-size: 0px;
          .title{
            font-size: 12px;
            color: rgb(7,17,27);
            vertical-align: top;
            margin-right: 24px;
          }
          .star{
            display: inline-block;
            .staritem{
              margin-right: 13px;
            }
          }
          .score{
            font-size: 12px;
            line-height: 36px;
            color: rgb(255,153,0);
          }
          .time{
            font-size: 12px;
            color: rgb(147,153,159);
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
    .ratingoption{
      padding: 36px 0 0 0;
    }
    .ratingwrap{
      padding: 0 36px;
      .list-item{
        width: 100%;
        padding: 0.42667rem 0;
        border-bottom: 1px solid rgba(7, 17, 27, 0.1);
        overflow:hidden;
        display:flex;
        .user-head{
          flex:0 0 80px;
          width: 80px;
          img{
            width: 56px;
            height: 56px;
            border-radius: 50%;
          }
        }
        .usercontent{
          flex: 1;
          .name{
            overflow:hidden;
            margin-bottom:8px;
            .username{
              float: left;
              font-size: 10px;
              color: rgb(7,17,27);
              line-height: 24px;
            }
            .time{
              float:right;
              font-size: 10px;
              color: rgb(147,153,159);
              font-weight: 200;
              line-height: 24px;
            }
          }
          .starstar{
            margin-bottom: 12px;
            .star{
              display: inline-block;
            }
            .sdtime{
              font-size: 10px;
              color: rgb(147,153,159);
              font-weight: 200;
              line-height: 24px;
              margin-left: 12px;
            }
          }
          .txt{
            font-size: 12px;
            color: rgb(7,17,27);
            line-height: 36px;
          }
          .recommend{
            margin-top: 16px;
            .icon{
              display: inline-block;
              width: 0.32rem;
              height: 0.32rem;
              &.active{
                background: url(height.png) no-repeat;
                background-size: cover;
              }
              &.off{
                background: url(off.png) no-repeat;
                background-size: cover;
              }
            }
            .foodz{
              color: rgb(147,153,158);
              font-size: 9px;
              display: inline-block;
              padding: 0 12px;
              margin-left: 16px;
              border: 1px solid rgba(7,17,27,0.3);
              line-height: 32px;
              margin-bottom: 16px;
            }
            
          }
        }
      }
    }
  }
</style>