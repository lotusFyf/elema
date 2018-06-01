<template>
	<div class="header">
		<div class="content-text">
			<div class="avatar">
				<img width="64" hight="64" :src="seller.avatar" alt="">
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				</div>
				<div class="description">
					{{seller.description}}/{{seller.deliveryTime}}分钟送达
				</div>
				<div v-if="seller.supports" class="support">
					<span class="icon" :class="classMap[seller.supports[0].type]"></span>
					<span class="text">{{seller.supports[0].description}}</span>
				</div>
				<div v-if="seller.supports" class="support-count" @click="showIt">
					<span class="count">{{seller.supports.length}}个</span>
					<i class="icon-keyboard_arrow_right"></i>
				</div>
			</div>
		</div>
		<div class="notice" @click="showIt">
			<span class="notice-pic"></span>
			<span class="notice-text">{{seller.bulletin}}</span>
			<i class="icon-keyboard_arrow_right"></i>
		</div>
		<div class="bgc">
			<img :src="seller.avatar" width="100%" height="100%">
		</div>
		<transition name="fade">
			<div class="showsince" v-show="isShow" >
				<div class="show-wrapper clearfix">
					<div class="show-main">
						<h1 class="name">{{seller.name}}</h1>
						<div class="star-wrapper">
							<v-star :size="48" :score="seller.score"></v-star>
						</div>
						<div class="title">
							<div class="line"></div>
							<div class="text">优惠信息</div>
							<div class="line"></div>
						</div>
						<ul v-if="seller.supports" class="supports">
							<li class="support-item" v-for="(item,index) in seller.supports">
								<span class="icon" :class="classMap[seller.supports[index].type]"></span>
								<span class="text">{{seller.supports[index].description}}</span>
							</li>
						</ul>
						<div class="title">
							<div class="line"></div>
							<div class="text">商家公告</div>
							<div class="line"></div>
						</div>
						<div class="bulletin">
							<p class="content">{{seller.bulletin}}</p>
						</div>
					</div>
				</div>
				<div class="show-close">
					<i class="icon-close" @click="closeShow"></i>
				</div>
			</div>
		</transition>
	</div>
</template>
<script>
	import star from '../star/star.vue'
	export default{
		props:{
			seller:{
				type:Object
			}
		},
		created(){
			this.classMap=['decrease','discount','special','invoice','guarantee']
		},
		data(){
			return{
				isShow:false
			}
		},
		methods:{
			showIt(){
				this.isShow=true
			},
			closeShow(){
				this.isShow=false
			}
		},
		components:{
			'v-star':star
		}
	}
</script>
<style>
	.header{
		position: relative;
		color: #Fff;
		background-color: rgba(7, 17, 27, 0.5);
		overflow: hidden;
	}
	.header .content-text{
		position: relative;
		padding: 24px 12px 18px 24px;
		font-size: 0;
	}
	.header .content-text .avatar{
		display: inline-block;
		font-size: 14px;
		vertical-align: top;
	}
	.header .content-text .avatar img{
		border-radius: 2px;
	}
	.header .content-text .content{
		display: inline-block;
		margin-left: 16px;
	}
	.header .content-text .content .title{
		margin: 2px 0 8px 0;
	}
	.header .content-text .content .title .brand{
		display: inline-block;
		width: 30px;
		height: 18px;
		background: url('brand@2x.png') no-repeat;
		background-size: 30px 18px;
		vertical-align: top;
	}
	.header .content-text .content .title .name{
		margin-left: 6px;
		font-size: 16px;
		line-height: 18px;
		font-weight: 700;
	}
	.header .content-text .content .description{
		margin-bottom: 10px;
		line-height: 12px;
		font-size: 12px;
	}
	.header .content-text .content .support .icon{
		display: inline-block;
		width: 12px;
		height: 12px;
		margin-right: 4px;
		vertical-align: top;
	}
	.header .content-text .content .support .decrease{
		background: url('decrease_1@2x.png');
		background-size: 12px 12px;
	}
	.header .content-text .content .support .discount{
		background: url('discount_1@2x.png');
		background-size: 12px 12px;
	}
	.header .content-text .content .support .guarantee{
		background: url('guarantee_1@2x.png');
		background-size: 12px 12px;
	}
	.header .content-text .content .support .invoice{
		background: url('invoice_1@2x.png');
		background-size: 12px 12px;
	}
	.header .content-text .content .support .special{
		background: url('special_1@2x.png');
		background-size: 12px 12px;
	}
	.header .content-text .content .support .text{
		line-height: 12px;
		font-size: 10px;
	/* 	color: rgb(255, 255, 255) */
	}
	.header .content-text .support-count{
		position: absolute;
		right: 12px;
		bottom: 14px;
		padding: 0 8px;
		height: 24px;
		line-height: 24px;
		border-radius: 14px;
		background-color: rgba(0, 0, 0, 0.2);
		text-align: center;
	}
	.header .content-text .support-count .count{
		font-size: 10px;
		vertical-align: top；
	}
	.header .content-text .support-count .icon-keyboard_arrow_right{
		font-size: 10px;
		line-height: 24px;
		margin-left: 2px;
	}
	.header .notice{
		position: relative;
		height: 28px;
		line-height: 28px;
		padding-right: 22px;
		padding-left: 12px;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		background-color: rgba(7, 17, 27, 0.2)
	}
	.header .notice .notice-pic{
		display: inline-block;
		width: 22px;
		height: 12px;
		background: url('bulletin@2x.png') no-repeat;
		background-size: 22px 12px;
		vertical-align: top;
		margin-top: 8px;
	}
	.header .notice .notice-text{
		font-size: 10px;
		margin: 0 4px;
		vertical-align: top;
	}
	.header .notice .icon-keyboard_arrow_right{
		position: absolute;
		font-size: 10px;
		right: 12px;
		top: 8px;
	}
	.header .bgc{
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: -1;
		filter: blur(10px);
	}
	.header .showsince{
		position: fixed;
		top: 0;
		left: 0;
		z-index: 100;
		width: 100%;
		height: 100%;
		overflow: auto;
		background-color: rgba(7, 17, 27, 0.8);
	}
	.fade-enter-active, .fade-leave-active {
	  transition: opacity .5s;
	}
	.fade-enter, .fade-leave{
	  opacity: 0;
	}
	.header .showsince .show-wrapper{
		min-height: 100%;
		width: 100%;
	}
	.header .showsince .show-wrapper .show-main{
		margin-top: 64px;
		padding-bottom: 64px;
	}
	.header .showsince .show-wrapper .show-main .name{
		line-height: 16px;
		text-align: center;
		font-size: 16px;
		font-weight: 700;
	}
	.header .showsince .show-wrapper .show-main .star-wrapper{
		margin-top: 18px;
		padding: 2px 0;
		text-align: center;
	}
	.header .showsince .show-close{
		position: relative;
		width: 32px;
		height: 32px;
		margin: -64px auto 0 auto;
		clear: both;
		font-size: 32px;
	}
	.header .showsince .title{
		width: 80%;
		margin: 28px auto 24px auto;
		display: flex;
	}
	.header .showsince .title .line{
		flex: 1;
		position: relative;
		top: -6px;
		border-bottom: 1px solid rgba(255,255,255,0.2);
	}
	.header .showsince .title .text{
		padding: 0 12px;
		font-size: 14px;
		font-weight: 700;
	}
	.header .showsince .supports{
		width: 80%;
		margin: 0 auto;
	}
	.header .showsince .supports .support-item{
		padding: 0 12px;
		margin-bottom: 12px;
		font-size: 0;
	}
	.header .showsince .supports:last-child{
		margin-bottom: 0;
	}
	.header .showsince .supports .support-item .icon{
		display: inline-block;
		width: 16px;
		height: 16px;
		vertical-align: top;
		margin-right: 6px;
		background-repeat: no-repeat;
	}
	.header .showsince .supports .support-item .decrease{
		background: url('decrease_2@2x.png');
		background-size: 16px 16px;
	}
	.header .showsince .supports .support-item .discount{
		background: url('discount_2@2x.png');
		background-size: 16px 16px;
	}
	.header .showsince .supports .support-item .guarantee{
		background: url('guarantee_2@2x.png');
		background-size: 16px 16px;
	}
	.header .showsince .supports .support-item .invoice{
		background: url('invoice_2@2x.png');
		background-size: 16px 16px;
	}
	.header .showsince .supports .support-item .special{
		background: url('special_2@2x.png');
		background-size: 16px 16px;
	}
	.header .showsince .supports .support-item .text{
		line-height: 12px;
		font-size: 12px;
	}
	.header .showsince .bulletin{
		width: 80%;
		margin: 0 auto;
	}
	.header .showsince .bulletin .content{
		padding: 0 12px;
		line-height: 24px;
		font-size: 12px;
	}
</style>