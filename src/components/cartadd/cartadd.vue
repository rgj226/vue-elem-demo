<template>
  <div class="cartadd" @click.stop="">
   <div class="cartdecrease" v-show="this.food.count>0" @click.stop.prevent="decrease"></div>
   <div class="cartaddcont" v-show="this.food.count>0"> {{this.food.count}}</div>
   <div class="cartadd" @click.stop.prevent="addcount"> </div>   
  </div>
</template>

<script type="text/ecmascript-6">
  //通过vue.set方法设置属性,引入vue
  import vue from 'vue'
  export default {
    data(){
      return{
        count:0
      }      
    },
    props:{
      food:{
        type:Object
      }
    },
    methods:{
   // vue.set设置属性方法，为food设施属性，为了心属性能与food关联，驱动数据更新
      addcount(){        
        if(!this.food.count){
          vue.set(this.food,'count',1)   //通过vue.set为food设置count属性，并设置值
        }else{          
          this.food.count++
        }
      },
      decrease(){  
        if(this.food.count){
          this.food.count--
          console.log(this.food.count)
        }
      }
    }
  };
</script>

<style lang="scss" rel="stylesheet/scss">
  .cartadd{
    width: 135px;
    .cartdecrease{
      display: inline-block;
      width: 42px;
      height: 42px;
      background: url(subtract.png) no-repeat;
      background-size: 42px 42px; 
      float: left;
      line-height: 40px;
    }
    .cartaddcont{
      display: inline-block;
      font-size: 12px;
      color: rgb(147,153,159);
      line-height: 40px;
      width: 48px;
      text-align: center;
      font-weight: 700;
    }
    .cartadd{
      display: inline-block;
      width: 42px;
      height: 42px;
      background: url(add.png) no-repeat;
      background-size: 42px 42px; 
      float: right;
      line-height: 40px;
    }
  }
  
</style>