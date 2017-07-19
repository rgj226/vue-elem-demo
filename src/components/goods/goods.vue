<template>
  <div class="goods-warp">
  	<div class="left-menus" ref="menus">
  	  <ul class="list-wrap">
  	  	<li v-for="(item,index) in goods" class="menu-list" :class="{'current':curindex===index}" @click="selectmenu(index,$event)">  	  	 
  	  	  <span class="text">  <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}</span>
  	  	</li>
  	  </ul>
  	</div>
  	<div class="goods-list" ref="goods">
  	  <ul> 
  	    <li v-for="item in goods" class="food food-list-hook" > 
  	      <h1 class="title">{{item.name}}</h1>
  	      <ul class="food-content">
  	       <li v-for="food in item.foods" class="food-item" @click="foodss(food,$event)">
	          <div class="imgbox" @click="showfn">
	           <img :src="food.icon" alt="" />
	          </div>
	          <div class="content" @click="showfn">
	           <h2 class="name">{{food.name}}</h2>
	           <p class="description">{{food.description}}</p>
	           <p class="notice"><span>月售{{food.sellCount}}份</span> <span>好评率{{food.rating}}%</span></p>
	           <p class="price"><span class="nowprice">￥{{food.price}}</span> <span v-show="food.oldPricer" class="oldprice">￥{{food.oldPricer}}</span></p>  	             	           
	          </div>	         
	          <div class="add">
              <cartadd :food="food"></cartadd>
            </div>
  	       </li>
  	      </ul>
  	    </li>
  	  </ul>
  	</div>
  	<shopcar :selectfood="selectfood" :psprice="seller.deliveryPrice" :min-price="seller.minPrice"></shopcar>
    <food :food="foodnotice" ref="fn"></food>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from "better-scroll";
  import shopcar from "../shopcar/shopcar";
  import cartadd from "../cartadd/cartadd";
  import food from "../food/food";
  export default {
    data(){
      return{
        goods:[],
        listheight:[],
        scrolly:0,
        foodnotice:{}
      };
    },
    props:{
      seller:{
        type:Object
      }
    },
    computed:{
      curindex(){
        for(let i=0;i<this.listheight.length;i++){ //判断滚动区间
          let height1=this.listheight[i];
          let height2=this.listheight[i + 1];
          if(!height2||(this.scrolly>=height1 && this.scrolly<height2)){
            return i
          }
        } 
        return 0;
      },    
      selectfood(){     //与购物车组件联动。
        let foods=[];  //遍历商品goods，找出所有的食物列表，从列表中遍历foods。
        this.goods.forEach((good)=>{
          good.foods.forEach((food)=>{
            if(food.count){    //与购物车组件联动时判断是否已经添加了商品
             foods.push(food)  
           }           
          });          
        });
        return foods
      }
    },
    created(){
      this.$http.get('api/goods').then(response => {
        response=response.body;      
        if(response.errno===0){
          this.goods=response.data  
          console.log(this.goods)
          this.$nextTick(function (){
           this.initscroll();
           this.calculateheight()
          })
        }
      })
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
//  自动去执行函数
//  mounted(){   
//    this.initscroll()
//  },
    methods:{
      selectmenu(index,event){     //indx当前点击的的节点索引值
        console.log(index)
        if(!event. _constructed){   //bscroll自有属性，用来判断是否手机
          return;
        }
        //思路：点击的时候获取到点击的节点，然后滚动到当前节点对应的节点
        let foodlist=this.$refs.goods.getElementsByClassName("food-list-hook")
        let el=foodlist[index]
        this.foodscroll.scrollToElement(el,300)
      },
      initscroll(){
        this.menuscroll=new BScroll(this.$refs.menus,{
          click: true
        });
        this.foodscroll=new BScroll(this.$refs.goods,{
          probeType: 3,  //实时监测滚动位置     
          click: true
        });    
        this.foodscroll.on('scroll', (pos) => {   //注意：这里是要按照官方的api的，event事件监听滚动。不是一个function
          this.scrolly=Math.abs(parseInt(pos.y))
        })
      },
      calculateheight(){
        //获取每个区块
        let foodlist=this.$refs.goods.getElementsByClassName("food-list-hook")
        let height =0;
        this.listheight.push(height);          //设置第一个区块的高度为0
        for(let i =0;i<foodlist.length;i++){  //遍历所有的区块
          height += foodlist[i].clientHeight;  //每个区块的高度总和
          this.listheight.push(height);        //获取到每个区块高度的数组
          //console.log(this.listheight)
        }       
      },
      foodss(food){
      this.foodnotice=food
      console.log(this.foodnotice)    
      },
      showfn(){
         this.$refs.fn.show()  //父组件调用子组件的方法
      }
    },
    components:{
      shopcar,
      cartadd,
      food
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  .goods-warp{
    display: flex;
    overflow: hidden;
    position: absolute;
    top: 351px;
    bottom: 92px;
    width: 100%;
    .left-menus{
      flex: 0 1 160px;
      height: 100%;
      background: #f3f5f7;
      .list-wrap{
        width: 160px;
      }
      .menu-list{
        display: table;
        padding: 0 24px;
        height: 108px;        
        line-height: 28px;
        &.current{
          background: #fff;
          .text{
            font-weight: 700;
            border: none;
          }
        }
        .text{
          font-size: 12px;
          display: table-cell;
          width: 120px;
          vertical-align: middle;
          text-align: center;
          border-bottom: 1px solid rgba(7,17,27,0.2);
        }
        .icon{
          display: inline-block;
          width: 24px;
          height: 24px;
          margin-right: 4px;
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
    .goods-list{
      flex: 1;
      height: 100%;
      .title{
        font-size: 12px;
        line-height: 52px;
        color: rgb(147,153,159);
        background: #f3f5f7;
        border-left:2px solid #d9dde1;
        padding-left: 28px;
      }
      .food-content{
        padding: 0 36px;
        .food-item{
          padding: 36px 0;
          border-bottom: 1px solid rgba(7,17,27,0.1);
          font-size:0;
          position: relative;
          &:last-child{
            border: none;
          }
          .add{
              position: absolute;
              right: 2px;
              bottom: 36px;
            }
          .imgbox{
            display: inline-block;
            padding-right: 20px;
            width: 114px;
            height: 114px;           
            img{
              width: 114px;
              height: 114px;
              border-radius: 6px;
            }
          }
          .content{
            display: inline-block;
            vertical-align: top;
            position: relative;
            .name{
              font-size: 14px;
              line-height: 28px;
              color: rgb(7,17,27);
              margin-bottom: 16px;
            }
            .description,.notice{
              font-size: 10px;
              line-height: 20px;
              color: rgb(147,153,159);
              margin-bottom: 16px;
              overflow: hidden;
              text-overflow: ellipsis;
              white-space: nowrap;
              width: 300px;
            }
            .price{
              font-size: 14px;
              color: #F01414;
              font-weight: 700;
              .oldprice{
                font-size: 10px;
                color: rgb(147,153,159);
                line-height: 48px;
                font-weight: 700;
                text-decoration: line-through;
              }
            }           
          }
        }
      }
    }
  }
</style>