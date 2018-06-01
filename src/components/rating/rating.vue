<template>
	<div class="rating" ref="rating">
		<div class="rating-content">
			<div class="overview">
				<div class="overview-left">
					<h1 class="score">{{seller.score}}</h1>
					<div class="title">综合评分</div>
					<div class="rank">高于周边商家{{seller.rankRate}}%</div>
				</div>
				<div class="overview-right">
					<div class="score-wrapper">
						<span class="title">服务态度</span>
						<star :size="36" :score="seller.serviceScore"></star>
						<span class="score">{{seller.serviceScore}}</span>
					</div>
					<div class="score-wrapper">
						<span class="title">商品评分</span>
						<star :size="36" :score="seller.foodScore"></star>
						<span class="score">{{seller.foodScore}}</span>
					</div>
					<div class="delivery-wrapper">
						<span class="title">送达时间</span>
						<span class="delivery">{{seller.deliveryTime}}分钟</span>
					</div>
				</div>
			</div>
			<split></split>
			<ratingselect :select-type="selectType" :only-content="onlyContent" :ratings="ratings"></ratingselect>
			<div class="rating-wrapper">
				<ul>
					<li v-for="rating in ratings" class="ratingItem" v-show="needShow(rating.rateType, rating.text)">
						<div class="avatar">
							<img :src="rating.avatar">
						</div>
						<div class="content">
							<h1 class="name">{{rating.username}}</h1>
							<div class="star-wrapper">
								<star :size='24' :score="rating.score"></star>
								<span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
							</div>
							<p class="text">{{rating.text}}</p>
							<div class="recommend" v-show="rating.recommend && rating.recommend.length">
				                <span class="icon-thumb_up"></span>
				                <span class="item" v-for="item in rating.recommend">{{item}}</span>
				            </div>
				            <div class="time">
				                {{rating.rateTime | formatDate}}
				            </div>
						</div>
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
	import BScroll from 'better-scroll'
	import star from '../star/star.vue'
	import ratingselect from '../ratingselect/ratingselect.vue'
	import split from '../split/split.vue'
	import {formatDate} from '../../js/data.js'
	const ALL = 2 ;
	export default{
		props:{
			seller:{
				type:Object
			}
		},
		data(){
			return{
				ratings:[],
				selectType:ALL,
				onlyContent:true
			}
		},
		methods:{
			needShow(type,text){
				if(this.onlyContent && !text){
					return false;
				}
				if(this.selectType === ALL){
					return true;
				}else{
					return type === this.selectType
				}
			}
		},
		components:{
			star,
			split,
			ratingselect,
		},
		created(){
			  var _this = this;
		      _this.$http.get('../static/data.json').then(function(response){
		      _this.ratings = response.data.ratings
		      _this.$nextTick(function(){
			      	_this.scroll = new BScroll(this.$refs.rating,{
			      	click:true
			      })
		      })
		    })
		},
		events:{
			'ratingtype.select'(type){
				this.selectType = type;
				this.$nextTick(function(){
					this.scroll.refresh();
				})
			},
			'content.toggle'(onlyContent){
				this.onlyContent = onlyContent;
				this.$nextTick(function(){
					this.scroll.refresh();
				})
			}
		},
		filters: {
	      formatDate(time) {
	        let date = new Date(time);
	        return formatDate(date, 'yyyy-MM-dd hh:mm');
	      }
	    },
	}
</script>

<style>
	.rating{
		position: absolute;
		top: 174px;
		bottom: 0;
		left: 0;
		width: 100%;
		overflow: hidden;
	}
	.rating .overview{
		display: flex;
		padding: 18px 0;
	}
	.rating .overview .overview-left{
		flex: 0 0 137px;
		width: 137px;
		border-right: 1px solid rgba(7, 17, 27, 0.1);
		text-align: center;
		padding: 6px 0;
	}
	.rating .overview .overview-left .score{
		line-height: 28px;
		font-size: 24px;
		color: rgb(255, 153, 0);
		margin-bottom: 6px;
	}
	.rating .overview .overview-left .title{
		margin-bottom: 8px;
		line-height: 12px;
		font-size: 12px;
		color: rgb(7, 17, 27);
	}
	.rating .overview .overview-left .rank{
		line-height: 10px;
		font-size: 10px;
		color: rgb(147,153,159);
	}
	.rating .overview .overview-right{
		flex: 1;
		padding-left: 24px;
	}
	.rating .overview .overview-right .score-wrapper{
		line-height: 18px;
		margin-bottom: 8px;
		font-size: 0 ;
	}
	.rating .overview .overview-right .score-wrapper .title{
		font-size: 12px;
		color: rgb(7, 17, 27);
		display: inline-block;
		vertical-align: top;
		line-height: 18px;
	}
	.rating .overview .overview-right .score-wrapper .star{
		display: inline-block;
		vertical-align: top;
		margin: 0 12px;
	}
	.rating .overview .overview-right .score-wrapper .score{
		display: inline-block;
		vertical-align: top;
		font-size: 12px;
		line-height: 18px;
		color: rgb(255, 153, 0)
	}
	.rating .overview .overview-right .delivery-wrapper{
		font-size: 0;
	}
	.rating .overview .overview-right .delivery-wrapper .title{
		line-height: 18px;
		font-size: 12px;
		color: rgb(7, 17, 27);
	}
	.rating .overview .overview-right .delivery-wrapper .delivery{
		margin-left: 12px;
		font-size: 12px;
		color: rgb(147, 153, 159);
	}
	.rating .rating-wrapper{
		padding: 0 18px;
	}
	.rating .rating-wrapper .ratingItem{
		display: flex;
		padding: 18px 0;
		border-bottom: 1px solid rgba(7, 17, 27, 0.1)
	}
	.rating .rating-wrapper .ratingItem .avatar{
		flex: 0 0 28px;
		width: 28px;
		margin-right: 12px;
	}
	.rating .rating-wrapper .ratingItem .avatar img{
		border-radius: 50%;
		width: 100%;
	}
	.rating .rating-wrapper .ratingItem .content{
		position: relative;
		flex: 1;
	}
	.rating .rating-wrapper .ratingItem .content .name{
		margin-bottom: 4px;
		line-height: 12px;
		font-size: 10px;
		color: rgb(7, 17, 27);
	}
	.rating .rating-wrapper .ratingItem .content .star-wrapper{
		margin-bottom: 6px;
		font-size: 0;
	}
	.rating .rating-wrapper .ratingItem .content .star-wrapper .star{
		display: inline-block;
		margin-right: 6px;
		vertical-align: top;
	}
	.rating .rating-wrapper .ratingItem .content .star-wrapper .delivery{
		display: inline-block;
		vertical-align: top;
		line-height: 12px;
		font-size: 10px;
		color: rgb(147, 153, 159);
	}
	.rating .rating-wrapper .ratingItem .content .text{
		margin-bottom: 8px;
		line-height: 18px;
		color: rgb(7,17,27);
	}
	.rating .rating-wrapper .ratingItem .content .recommend{
		line-height: 16px;
		font-size: 0;
	}
	.rating .rating-wrapper .ratingItem .content .recommend .icon-thumb_up{
		display: inline-block;
		margin: 0 8px 4px 0;
		font-size: 9px;
		color: rgb(0, 160, 220);
	}
	.rating .rating-wrapper .ratingItem .content .recommend .item{
		display: inline-block;
		margin: 0 8px 4px 0;
		font-size: 9px;
		padding: 0 6px;
		border: 1px solid rgba(7, 17, 27, 0.1);
		border-radius: 1px;
		color: rgb(147, 153, 159);
		background: #fff;
	}
	.rating .rating-wrapper .ratingItem .content .time{
		position: absolute;
		top: 0;
		right: 0;
		line-height: 12px;
		font-size: 10px;
		color: rgb(147, 153, 159);
	}
</style>