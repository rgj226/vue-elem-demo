<template>
  <div class="ratingselect">
    <div class="ratingtype">
      <div class="btnwrap">
        <span @click="select(2)" class="block all" :class="{'active':selecttype==2}">{{desc.all}} <span class="count">{{ratings.length}}</span></span>
        <span @click="select(0)" class="block positive" :class="{'active':selecttype==0}">{{desc.positive}} <span class="count">2</span> </span>
        <span @click="select(1)" class="block negative" :class="{'active':selecttype==1}">{{desc.negative}} <span class="count">17</span></span>
      </div>      
    </div>
    <div class="switch" @click="switchtogo">
      <span class="icon" :class="{'active':onlycount==true}"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
  
</template>

<script type="text/ecmascript-6">
  const positive = 0;   //评价选择的选择的三种状态。正常，不推荐/不满意，所有
  const negative = 1;
  const all = 2;
  export default {
   
    props:{
      ratings:{        //评价内容
        type:Array,
        default(){
          return []
        }
      },
      selecttype:{  //选择类型，依据类型改变是推荐，吐槽，满意不满意的内容等
        type:Number,
        default:0 
      },
      onlycount:{  //是否只看有内容，默认关闭
        type:Boolean,
        default:false       
      },
      desc:{
        type:Object ,
        default(){
          return{
            all:'全部',
            positive:"满意",
            negative:'不满意'
          }
        }        
      }
    },
    methods:{
      select(type){
//      this.selecttype=type;
//      this.$root.eventHub.$emit('ratingtype',type)
        this.$emit('ratingtype',type)    //vue自定义事件
      },
      switchtogo(){        
        //this.onlycount=!this.onlycount   //在子组件中不能去修改父组件传递过来的prop
        this.$root.eventHub.$emit('onlynum',this.onlycount)
      }
    }
  };
</script>

<style lang="scss" rel="stylesheet/scss">
  .ratingselect{
    .ratingtype{
      padding: 0 36px;
      .btnwrap{
        padding-bottom: 36px;
        border-bottom: 1px solid rgba(7,17,27,0.3);
        .block{
          display: inline-block;
          padding: 16px 24px;
          margin-right: 16px;
          font-size: 12px;
          color: rgb(77,85,93);
          line-height: 32px;
          text-align: center;
          border-radius: 4px;
          &.all{
            background: rgb(0,160,220);
          }
          &.positive{
            background: rgba(0,160,220,0.4);
          }
          &.negative{
            background: rgba(77,85,93,0.4);
          }
          &.active{
            color: #FFFFFF;
            .count{
              color: #FFFFFF;
            }
          }
        }
        .count{
          font-size: 8px;
          color: rgb(77,85,93);
        }
      }
      
    }
    .switch{
      margin-top: 24px;
      padding: 0 36px 24px 36px;
      border-bottom: 1px solid rgba(7,17,27,0.3);
      .icon{
        display: inline-block;
        width: 48px;
        height: 48px;
        background-image: url(off.png);
        background-repeat: no-repeat;
        background-size: 48px 48px;
        line-height: 48px;
        &.active{
          background-image: url(on.png);
          background-repeat: no-repeat;
          background-size: 48px 48px;
        }
      }
      .text{
        font-size: 12px;
        color: rgb(147,153,159);
        line-height: 48px;
        vertical-align: top;
      }
    }
  }
</style>