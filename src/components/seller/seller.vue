<template>
	<div class="seller" ref="seller">
		<div class="seller-content">
			<div class="overview">
				<h1 class="title">{{seller.name}}</h1>
				<div class="desc">
					<star :size="36" :score="seller.score"></star>
					<span class="text">({{seller.ratingCount}})</span>
					<span class="text">月售{{seller.sellCount}}单</span>
				</div>
				<ul class="remark">
					<li class="block">
						<h2>起送价</h2>
						<div class="content">
							<span class="stress">{{seller.minPrice}}</span>元
						</div>
					</li>
					<li class="block">
						<h2>商家配送</h2>
						<div class="content">
							<span class="stress">{{seller.deliveryPrice}}</span>元
						</div>
					</li>
					<li class="block">
						<h2>平均配送时间</h2>
						<div class="content">
							<span class="stress">{{seller.deliveryTime}}</span>分钟
						</div>
					</li>
				</ul>
			</div>
			<split></split>
			<div class="bulletin">
				<h1 class="title">公告与活动</h1>
				<div class="content-wrapper">
					<p class="content">{{seller.bulletin}}</p>
				</div>
				<ul v-if="seller.supports" class="supports">
					<li class="support-item" v-for="(item,index) in seller.supports">
						<span class="icon" :class="classMap[seller.supports[index].type]"></span>
						<span class="text">{{seller.supports[index].description}}</span>
					</li>
				</ul>
			</div>
			<split></split>
			<div class="pics">
				<h1 class="title">商家实景</h1>
				<div class="pic-wrapper" ref="picWrapper">
					<ul class="pic-list" ref="picList">
						<li class="pic-item" v-for="pic in seller.pics">
							<img :src="pic" width="120" height="90">
						</li>
					</ul>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import star from '../star/star.vue'
	import split from '../split/split.vue'
	import BScroll from 'better-scroll'
	export default{
		props:{
			seller:{
				type:Object
			}
		},
		components:{
			star,
			split
		},
		created(){
			this.classMap=['decrease','discount','special','invoice','guarantee']
		},
		mounted(){
			this.scroll = new BScroll(this.$refs.seller,{
				click:true
			})
			if(this.seller.pics){
				var picWidth = 120;
				var margin = 6;
				var width = (picWidth + margin) * this.seller.pics.length - margin;
				this.$refs.picList.style.width = width + 'px';
				this.scroll = new BScroll(this.$refs.picWrapper,{
					scrollX:true,
					eventPassthrough:'vertical'
				})
			}
		},
	}
</script>

<style>
	.seller{
		position: absolute;
		top: 174px;
		bottom: 0;
		left: 0;
		width: 100%;
		overflow: hidden;
	}
	.seller .overview{
		padding: 18px;
	}
	.seller .overview .title{
		margin-bottom: 8px;
		line-height: 14px;
		font-size: 14px;
		color: rgb(7, 17, 27);
	}
	.seller .overview .desc{
		padding-bottom: 18px;
		border-bottom: 1px solid rgba(7, 17, 27, 0.1);
		font-size: 0;
	}
	.seller .overview .desc .star{
		display: inline-block;
		margin-right: 8px;
		vertical-align: top;
	}
	.seller .overview .desc .text{
		margin-right: 12px;
		line-height: 18px;
		display: inline-block;
		vertical-align: top;
		font-size: 10px;
		color: rgb(77, 85, 93)
	}
	.seller .overview .remark{
		display: flex;
		padding-top: 18px;
	}
	.seller .overview .remark .block{
		flex: 1;
		text-align: center;
		border-right: 1px solid rgba(7, 17, 27, 0.1);
	}
	.seller .overview .remark .block:last-child{
		border: none;
	}
	.seller .overview .remark .block h2{
		margin-bottom: 4px;
		line-height: 10px;
		font-size: 10px;
		color: rgb(147, 153, 159);
	}
	.seller .overview .remark .block .content{
		line-height: 24px;
		font-size: 10px;
		color: rgb(7, 17, 27)
	}
	.seller .overview .remark .block .content .stress{
		font-size: 24px;
	}
	.seller .bulletin{
		padding: 18px 18px 0 18px;
	}
	.seller .bulletin .title{
		margin-bottom: 8px;
		line-height: 14px;
		font-size: 14px;
		color: rgb(7, 17, 27);
	}
	.seller .bulletin  .content-wrapper{
		padding: 0 12px 16px 12px;
		border-bottom: 1px solid rgba(7, 17, 27, 0.1);
	}
	.seller .bulletin  .content-wrapper .content{
		line-height: 24px;
		font-size: 12px;
		color: rgb(240, 20, 20)
	}
	.seller .bulletin  .supports .support-item{
		padding: 16px 12px;
		border-bottom: 1px solid rgba(7, 17, 27, 0.1);
		font-size: 0;
	}
	.seller .bulletin  .supports .support-item .icon{
		display: inline-block;
		width: 16px;
		height: 16px;
		vertical-align: top;
		margin-right: 6px;
		background-size: 16px 16px;
		background-repeat: no-repeat;
	}
	.seller .bulletin  .supports .support-item .decrease{
		background: url('decrease_4@2x.png');
		background-size: 16px 16px;
	}
	.seller .bulletin  .supports .support-item .discount{
		background: url('discount_4@2x.png');
		background-size: 16px 16px;
	}
	.seller .bulletin  .supports .support-item .guarantee{
		background: url('guarantee_4@2x.png');
		background-size: 16px 16px;
	}
	.seller .bulletin  .supports .support-item .invoice{
		background: url('invoice_4@2x.png');
		background-size: 16px 16px;
	}
	.seller .bulletin  .supports .support-item .special{
		background: url('special_4@2x.png');
		background-size: 16px 16px;
	}
	.seller .bulletin  .supports .support-item .text{
		line-height: 16px;
		font-size: 12px;
		color: rgb(7, 17, 27);
	}
	.seller .bulletin  .supports .support-item:last-child{
		border: none;
	}
	.seller .pics{
		padding: 18px;
	}
	.seller .pics .title{
		margin-bottom: 12px;
		line-height: 14px;
		color: rgb(7, 17, 27);
		font-size: 14px;
	}
	.seller .pics .pic-wrapper{
		width: 100%;
		overflow: hidden;
		white-space: nowrap;
	}
	.seller .pics .pic-wrapper .pic-list{
		font-size: 0;
	}
	.seller .pics .pic-wrapper .pic-list .pic-item{
		display: inline-block;
		margin-right: 6px;
		width: 120px;
		height: 90px;
	}
	.seller .pics .pic-wrapper .pic-list:last-child{
		margin: 0;
	}
</style>