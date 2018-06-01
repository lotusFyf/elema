<template>
	<div class="goods">
		<div class="menu" ref="menuWrapper">
			<ul>
				<li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}" @click="clickmenu(index,$event)">
					<span class="text">
						<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}</span>
				</li>
			</ul>
		</div>
		<div class="foods" ref="foodsWrapper">
			<ul>
				<li v-for="item in goods" class="foodList foodList-hook">
					<h1 class="title">{{item.name}}</h1>
					<ul>
						<li @click="selectFood(food,$event)"  v-for="food in item.foods" class="foodItem">
							<div class="icon">
								<img :src="food.icon" >
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="miaoshu">{{food.description}}</p>
								<div class="extra">
									<span>月售{{food.sellCount}}份</span>
									<span>好评率{{food.rating}}%</span>
								</div>
								<div class="price">
									<span class="now">￥{{food.price}}</span>
									<span v-show="food.oldPrice" class="older">￥{{food.oldPrice}}</span>
								</div>
								<div class="btn-wrapper">
									<buybtn :food="food"></buybtn>
								</div>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
		<shopping :foods-num="selectFoods()" :price1="seller.deliveryPrice" :price2="seller.minPrice"></shopping>
		<food :food="selectedFood" ref="food"></food>
	</div>
</template>

<script>
	import BScroll from 'better-scroll'
	import shopping from '../shopping/shopping.vue'
	import buybtn from '../buybtn/buybtn.vue'
	import food from '../food/food.vue'
	export default{
		props:{
			seller:{
				type:Object
			}
		},
		 data(){
		    return{
		      goods:[],
		      listHeight:[],
		      scrollY: 0,
		      selectedFood:{},
		    }
		},
		computed:{
			currentIndex(){
				for(var i=0;i<this.listHeight.length;i++){
					var height1 = this.listHeight[i]
					var height2 = this.listHeight[i+1]
					if(!height2 || (this.scrollY>=height1 && this.scrollY<height2)){
						return i
					}
				}
				return 0
			}
		},
		created(){
			this.classMap=['decrease','discount','special','invoice','guarantee']
		      var _this = this;
		      _this.$http.get('../static/data.json').then(function(response){
		      _this.goods = response.data.goods
		      _this.$nextTick(function(){
		      	_this._initScroll()
		      	_this.mathHeight()
		      })
		    })
		  },
		methods:{
			clickmenu(index,event){
				if(!event._constructed){
					return
				}
				var foodLists = this.$refs.foodsWrapper.getElementsByClassName('foodList-hook')
				var el = foodLists[index]
				this.foodsScroll.scrollToElement(el,300)
			},
			_initScroll(){
				this.meunScroll = new BScroll(this.$refs.menuWrapper,{
					click:true
				})
				this.foodsScroll = new BScroll(this.$refs.foodsWrapper,{
					probeType:3,
					click:true
				})
				this.foodsScroll.on('scroll',function(pos){
					this.scrollY = Math.abs(Math.round(pos.y))
				})
			},
			mathHeight(){
				var foodLists = this.$refs.foodsWrapper.getElementsByClassName('foodList-hook')
				var height = 0
				this.listHeight.push(height)
				for(var i=0;i<foodLists.length;i++){
					var item = foodLists[i]
					height +=item.clientHeight
					this.listHeight.push(height)
				}
			},
			selectFoods(){
				var foods = []
				this.goods.forEach(function(good){
					good.foods.forEach(function(food){
						if(food.count){
							foods.push(food);
						}
					})
				})
				return foods
			},
			selectFood(food,event){
				if(!event._constructed){
					return
				}
				this.selectedFood = food ;
				this.$refs.food.show()
			}
		},
		components:{
			shopping,
			buybtn,
			food,
		}
	}
</script>

<style>
	.goods{
		display: flex;
		position: absolute;
		top: 174px;
		width: 100%;
		bottom: 46px;
		overflow: hidden;
	}
	.goods .menu{
		flex: 0 0 80px;
		width: 80px;
		background: #f3f5f7;
	}
	.goods .menu ul .menu-item{
		display: table;
		height: 54px;
		width: 56px;
		line-height: 14px;
		padding: 0 12px;
	}
	.goods .menu ul .current{
		position: relative;
		z-index: 10;
		background: #fff;
		margin-top: -1px;
	}
	.goods .menu ul .current .text{
		border-bottom: 0px;
		font-weight: 700;
	}
	.goods .menu ul .menu-item .icon{
		display: inline-block;
		width: 12px;
		height: 12px;
		margin-right: 2px;
		vertical-align: top;
	}
	.goods .menu ul .menu-item .decrease{
		background: url('../header/decrease_3@2x.png');
		background-size: 12px 12px;
	}
	.goods .menu ul .menu-item .discount{
		background: url('../header/discount_3@2x.png');
		background-size: 12px 12px;
	}
	.goods .menu ul .menu-item .guarantee{
		background: url('../header/guarantee_3@2x.png');
		background-size: 12px 12px;
	}
	.goods .menu ul .menu-item .invoice{
		background: url('../header/invoice_3@2x.png');
		background-size: 12px 12px;
	}
	.goods .menu ul .menu-item .special{
		background: url('../header/special_3@2x.png');
		background-size: 12px 12px;
	}
	.goods .menu ul .menu-item .text{
		font-size: 12px;
		display: table-cell;
		width: 56px;
		vertical-align: middle;
		border-bottom: 1px solid rgba(7, 17, 27, 0.2)
	}
	.goods .foods{
		flex: 1;
	}
	.goods .foods .title{
		padding-left: 14px;
		height: 26px;
		line-height: 26px;
		border-left: 1px solid #d9dde1;
		font-size: 12px;
		color: rgb(147,153,159);
		background: #f3f5f7;
	}
	.goods .foods .foodItem{
		display: flex;
		margin: 18px;
		border-bottom: 1px solid rgba(7, 17, 27 ,0.1)
	}
	.goods .foods:last-child{
		border-bottom: 0px;
		margin-bottom: 0;
	}
	.goods .foods .foodItem .icon{
		flex: 0 0 57px;
		margin-right: 10px;
	}
	.goods .foods .foodItem .icon img{
		width: 100%;
	}
	.goods .foods .foodItem .content{
		flex: 1;
		position: relative;
	}
	.goods .foods .foodItem .content .name{
		margin: 2px 0 8px 0;
		height: 14px;
		line-height: 14px;
		font-size: 14px;
		color: rgb(7,17,27)
	}
	.goods .foods .foodItem .content .miaoshu{
		margin-bottom: 8px;
		line-height: 10px;
		font-size: 10px;
		color: rgb(147,153,159);
	}
	.goods .foods .foodItem .content .extra{
		font-size: 0;
		line-height: 10px;
		color: rgb(147,153,159);
	}
	.goods .foods .foodItem .content .extra span{
		margin-right: 12px;
		font-size: 10px;
	}
	.goods .foods .foodItem .content .price{
		font-weight: 700;
		line-height: 24px;
	}
	.goods .foods .foodItem .content .price .now{
		margin-right: 8px;
		font-size: 14px;
		color: rgb(240,20,20)
	}
	.goods .foods .foodItem .content .price .older{
		text-decoration: line-through;
		font-size: 10px;
		color: rgb(147, 153, 159)
	}
	.goods .foods .foodItem .content .btn-wrapper{
		position: absolute;
		right: 0;
		bottom: 12px;
	}
</style>