<template>
  <div class="star-warp" :class="strtType">
    <span v-for="itemclass in itemclasss " :class="itemclass" class="staritem" ></span>
  </div>
</template>

<script type="text/ecmascript-6">
  const LENGHT=5
  const CLS_ON="on"
  const CLS_HALF="half"
  const CLS_OFF="off"
  
  export default {
    props:{
      size:{
        type:Number
      },
      score:{
         type:Number
      }
    },
    computed:{
      strtType(){
        return "star-" + this.size
      },
      itemclasss(){
        let result=[];
        let score=Math.floor(this.score*2)/2;
        let hasxx =score%1!==0;
        let integer=Math.floor(score);
        for(let i=0;i<integer;i++){
          result.push(CLS_ON)
        }
        if(hasxx){
          result.push(CLS_HALF)
        }
        while(result.length<LENGHT){
          result.push(CLS_OFF)
        }
        return result;
      }
    }
  };
</script>

<style lang="scss" rel="stylesheet/scss">
  .star-warp{
    .staritem{
      display: inline-block;
      background-repeat: no-repeat;
      }
      &.star-48{
        .staritem{
          width: 40px;
          height: 40px;
          margin-right: 44px;
          background-size: 40px 40px;
          &:last-child{
            margin-right: 0;
          }
          &.on{
            background-image: url(star48_on@2x.png);
          }
          &.half{
           background-image: url(star48_half@2x.png); 
          }
          &.off{
            background-image: url(star48_off@2x.png);
          }
        }
      }
      &.star-36{
        .staritem{
          width: 30px;
          height: 30px;
          margin-right: 32px;
          background-size: 30px 30px;
          &:last-child{
            margin-right: 0;
          }
          &.on{
            background-image: url(star36_on@2x.png);
          }
          &.half{
           background-image: url(star36_half@2x.png); 
          }
          &.off{
            background-image: url(star36_off@2x.png);
          }
        }
      }
      &.star-24{
        .staritem{
          width: 20px;
          height: 20px;
          margin-right: 6px;
          background-size: 20px 20px;
          &:last-child{
            margin-right: 0;
          }
          &.on{
            background-image: url(star24_on@2x.png);
          }
          &.half{
           background-image: url(star24_half@2x.png); 
          }
          &.off{
            background-image: url(star24_off@2x.png);
          }
        }
      }
  }
</style>