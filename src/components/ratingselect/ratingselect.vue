<template>
	<div class="ratingselect">
		<div class="ratingType">
			<span @click="select(2,$event)" class="block positive" :class="{'active':selectType1===2}">{{desc.all}}
				<span class="count">{{ratings.length}}</span>
			</span>
			<span @click="select(0,$event)" class="block positive" :class="{'active':selectType1===0}">{{desc.positive}}
				<span class="count">{{positives.length}}</span>
			</span>
			<span @click="select(1,$event)"  class="block negative" :class="{'active1':selectType1===1}">{{desc.negative}}
				<span class="count">{{negatives.length}}</span>
			</span>
		</div>
		<div @click="togglecontent" class="switch" :class="{'on':onlyContent1}">
			<span class="icon-check_circle"></span>
			<span class="text">只看有内容的评价</span>
		</div>
	</div>
</template>

<script>
	const POSITIVE = 0;
	const NEGATIVE = 1;
	const ALL = 2 ;
	export default{
		props:{
			ratings:{
				type:Array,
				default(){
					return[]
				}
			},
			selectType:{
				type:Number,
				default: ALL
			},
			onlyContent:{
				type:Boolean,
				default:false
			},
			desc:{
				type:Object,
				default(){
					return{
						all:'全部',
						positive:'满意',
						negative:'不满意',
					}
				}
			}
		},
		data(){
			return{
				selectType1:this.selectType,
				onlyContent1:this.onlyContent,
			}
		},
		methods:{
			select(type,event){
				if(!event._constructed){
					return;
				}
				this.selectType1 = type ;
				this.$emit('ratingtype.select', type);
			},
			togglecontent(event){
				if(!event._constructed){
					return;
				}
				this.onlyContent1 = !this.onlyContent1
				this.$emit('content.toggle',this.onlyContent1)
			}
		},
		computed:{
			positives(){
				return this.ratings.filter(function(rating){
					return rating.rateType === POSITIVE;
				})
			},
			negatives(){
				return this.ratings.filter(function(rating){
					return rating.rateType === NEGATIVE;
				})
			}
		}
	}
</script>

<style>
	.ratingselect .ratingType{
		padding: 18px 0;
		margin: 0 18px;
		border-bottom: 1px solid rgba(7, 17, 27, 0.1);
		font-size: 0;
	}
	.ratingselect .ratingType .block{
		display: inline-block;
		padding: 8px 12px;
		margin-right: 8px;
		border-radius: 1px;
		color: rgb(7, 85, 93);
		font-size: 12px;
		line-height: 16px;
	}
	.ratingselect .ratingType .active{
		color: #fff;
	}
	.ratingselect .ratingType .block .count{
		font-size: 8px;
		margin-left: 2px;
	}
	.ratingselect .ratingType .positive{
		background: rgba(0, 160, 220,0.2);
	}
	.ratingselect .ratingType .active{
		background: rgb(0, 160, 220)
	}
	.ratingselect .ratingType .negative{
		background: rgba(77, 85, 93, 0.2);
	}
	.ratingselect .ratingType .active1{
		background: rgb(77, 85, 93);
		color: #fff;
	}
	.ratingselect .switch{
		padding: 12px 18px;
		line-height: 24px;
		font-size: 0;
		border-bottom: 1px solid rgba(7, 17, 27, 0.1);
		color: rgb(147, 153, 159);
	}
	.ratingselect .on .icon-check_circle{
		color: #00c850;
	}
	.ratingselect .switch .icon-check_circle{
		font-size: 24px;
		margin-right: 4px;
		display: inline-block;
		vertical-align: top;
	}
	.ratingselect .switch .text{
		font-size: 12px;
		display: inline-block;
		vertical-align: top;
	}
</style>