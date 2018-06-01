<template>
	<div>
		<div class="shopping">
			<div class="content" @click="toggleList">
				<div class="left">
					<div class="logo-box">
						<div class="logo" :class="{'hightlight':totalCount>0}">
							<i class="icon-shopping_cart" :class="{'hightlight':totalCount>0}"></i>
						</div>
						<div class="num" v-if="totalCount!==0">{{totalCount}}</div>
					</div>
					<div class="money" :class="{'hightlight':totalPrice>0}">￥{{totalPrice}}</div>
					<div class="sendprice">另需配送费￥{{price1}}元</div>
				</div>
				<div class="right" @click.stop="paymoney">
					<div class="pay" :class="payClass">
						{{pay}}
					</div>
				</div>
			</div>
	<!-- 		<transition name="drop">
				<div class="ball-container">
					<div v-for="ball in balls" v-show="ball.show" class="ball">
						<div class="inner"></div>
					</div>
				</div>
			</transition> -->
			<transition name="fold">
				<div class="shoplist" v-show="listshow">
					<div class="listhead">
						<h1 class="title">购物车</h1>
						<span class="empty" @click="empty">清空</span>
					</div>
					<div class="listcontent" ref="listcontent">
						<ul>
							<li class="food" v-for="food in foodsNum">
								<span class="name">{{food.name}}</span>
								<div class="price">
									<span>￥{{food.price*food.count}}</span>
								</div>
								<div class="buybtn-wrapper">
									<buybtn :food="food"></buybtn>
								</div>
							</li>
						</ul>
					</div>
				</div>
			</transition>
		</div>
		<transition name="fade">
			<div class="listmast" v-show="listshow" @click="hideList"></div>
		</transition>
	</div>
</template>

<script>
	import BScroll from 'better-scroll'
	import buybtn from '../buybtn/buybtn.vue'
	export default{
		props:{
			price1:{
				type:Number,
				default:0
			},
			price2:{
				type:Number,
				default:0
			},
			foodsNum:{
				type:Array,
				default(){
					return []
				}
			}
		},
		data(){
			return{
				fold:false
			}
		},
		computed:{
			totalPrice(){
				var totle =0
				this.foodsNum.forEach(function(food){
					totle +=food.price*food.count;
				})
				return totle
			},
			totalCount(){
				var count = 0
				this.foodsNum.forEach(function(food){
					count +=food.count;
				})
				return count
			},
			pay(){
				if(this.totalPrice === 0){
					return '￥'+this.price2+'元起送'
				}else if(this.totalPrice<this.price2){
					var diff = this.price2 - this.totalPrice
					return '还差￥'+diff+'元起送'
				}else{
					return '去结算'
				}
			},
			payClass(){
				if(this.totalPrice<this.price2){
					return 'not-enough'
				}else{
					return 'enough'
				}
			},
			listshow(){
				if(!this.totalCount){
					this.fold = true
					return false
				}
				var show =!this.fold
				if(show){
					this.$nextTick(function(){
						if(!this.scroll){
							this.scroll = new BScroll(this.$refs.listcontent, {
							click:true
						})
						}else{
							this.scroll.refresh()
						}
					})
				}
				return show
			}
		},
		components:{
			buybtn
		},
		methods:{
			toggleList(){
				if(!this.totalCount){
					return
				}
				this.fold=!this.fold
			},
			empty(){
				this.foodsNum.forEach(function(food){
					food.count=0
				})
			},
			hideList(){
				this.fold = true
			},
			paymoney(){
				if(this.totalPrice<this.price2){
					return
				}
				window.alert('支付'+this.totalPrice+'元')
			}
		}
	}
</script>

<style>
	.shopping{
		position: fixed;
		left: 0;
		bottom: 0;
		z-index: 50;
		height: 48px;
		width: 100%;
	}
	.shopping .content {
		display: flex;
		background: #141d27;
	}
	.shopping .content .left{
		flex: 1;
	}
	.shopping .content .left .logo-box{
		display: inline-block;
		position: relative;
		top: -10px;
		margin: 0 12px;
		padding: 6px;
		width: 56px;
		height: 56px;
		box-sizing: border-box;
		vertical-align: top;
		border-radius: 50%;
		background: #141d27;
	}
	.shopping .content .left .logo-box .logo{
		width: 100%;
		height: 100%;
		border-radius: 50%;
		background: #2b343c;
		text-align: center;
	}
	.shopping .content .left .logo-box .hightlight{
		background: rgb(0,160,220)
	}
	.shopping .content .left .logo-box .num{
		position: absolute;
		top: 0;
		right: 0;
		width: 24px;
		height: 16px;
		line-height: 16px;
		text-align: center;
		border-radius: 16px;
		font-size: 9px;
		font-weight: 700;
		color: #fff;
		background: rgb(240, 20, 20);
		box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
	}
	.shopping .content .left .logo-box .logo .icon-shopping_cart{
		line-height: 44px;
		font-size: 24px;
		color: #80858a;
	}
	.shopping .content .left .logo-box .logo .hightlight{
		color: #fff;
	}
	.shopping .content .left .money{
		display: inline-block;
		vertical-align: top;
		line-height: 24px;
		margin-top: 12px;
		padding-right: 12px;
		box-sizing: border-box;
		border-right: 1px solid rgba(255, 255, 255, 0.1);
		font-size: 16px;
		font-weight: 700;
		color: rgba(255,255,255,0.4)
	}
	.shopping .content .left .hightlight{
		color: #fff;
	}
	.shopping .content .left .sendprice{
		display: inline-block;
		vertical-align: top;
		line-height: 24px;
		margin: 12px 0 0 12px;
		color: rgba(255,255,255,0.4);
		font-size: 10px;
	}
	.shopping .content .right{
		flex: 0 0 105px;
		width: 105px;
	}
	.shopping .content .right .pay{
		height: 48px;
		line-height: 48px;
		text-align: center;
		font-size: 12px;
		color: rgba(255,255,255,0.4);
		font-weight: 700;
		background: #2b333b;
	}
	.shopping .content .right .notenough{
		background: #2b333b;
	}
	.shopping .content .right .enough{
		background: #00b43c;
		color: #fff;
	}
/* 	.shopping .ball-container .ball{
		position: fixed;
		left: 32px;
		bottom: 22px;
		z-index: 200;
	}
	.inner{
		width: 16px;
		height: 16px;
		border-radius: 50%;
		background: rgb(0, 160, 220)
	} */
/* 	.drop-enter-active, .drop-leave-active {
	  transition: opacity .4s;
	}
	.drop-enter, .drop-leave-to{
	  opacity: 0;
	} */
	.shopping .shoplist{
		position: absolute;
		top: 0;
		left: 0;
		z-index: -1;
		width: 100%;
		transform: translate3d(0, -100%, 0)
	}
	.fold-enter-active, .fold-leave-active {
	  transition: all 5s;
	  transform: translate3d(0, -100%, 0);
	}
	.fold-enter, .fold-leave-to{
	  transform: translate3d(0, 0, 0);
	}
	.shopping .shoplist .listhead{
		height: 40px;
		line-height: 40px;
		padding: 0 18px;
		background: #f3f5f7;
		border-bottom: 1px solid rgba(7, 17, 27, 0.1)
	}
	.shopping .shoplist .listhead .title{
		float: left;
		font-size: 14px;
		color: rgb(7, 17, 27);
	}
	.shopping .shoplist .listhead .empty{
		float: right;
		font-size: 12px;
		color: rgb(0, 160, 220)
	}
	.shopping .shoplist .listcontent{
		padding: 0 18px;
		max-height: 217px;
		background: #fff;
		overflow: hidden;
	}
	.shopping .shoplist .listcontent .food{
		position: relative;
		padding: 12px 0;
		box-sizing: border-box;
		border-bottom: 1px solid rgba(7, 17, 27, 0.1)
	}
	.shopping .shoplist .listcontent .food .name{
		line-height: 24px;
		font-size: 14px;
		color: rgb(7, 17, 27);
	}
	.shopping .shoplist .listcontent .food .price{
		position: absolute;
		right: 90px;
		bottom: 12px;
		line-height: 24px;
		font-size: 14px;
		font-weight: 700;
		color: rgb(240, 20, 20)
	}
	.shopping .shoplist .listcontent .food .buybtn-wrapper{
		position: absolute;
		right: 0;
		bottom: 6px;
	}
	.listmast{
		position: fixed;
		top: 0;
		left: 0;
		height: 100%;
		width: 100%;
		z-index: 40;
		background: rgba(7,17,27,0.6);
		filter: blur(10px);
	}
	.fade-enter-active, .fade-leave-active {
	  transition: opacity .5s;
	}
	.fade-enter, .fade-leave{
	  opacity: 0;
	}
</style>