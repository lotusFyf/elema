<template>
	<div class="buybtn">
	<!-- 	<transition name="fade"> -->
			<div class="decrease icon-remove_circle_outline" v-show="food.count>0" @click.stop.prevent="decreasesince"></div>
			<div class="count" v-show="food.count>0">{{food.count}}</div>
			<div class="add icon-add_circle" @click.stop.prevent="addsince"></div>
	<!-- 	</transition> -->
	</div>
</template>

<script>
	import Vue from 'vue'
	export default{
		props:{
			food:{
				type:Object
			}
		},
		methods:{
			addsince(event){
				if(!event._constructed){
					return;
				}
				if(!this.food.count){
					Vue.set(this.food,'count',1)
				}else{
					this.food.count ++;
				}
			},
			decreasesince(event){
				if(!event._constructed){
					return;
				}
				if(this.food.count){
					this.food.count --;
				}
			}
		}
	}
</script>

<style>
	.buybtn{
		font-size: 0;
	}
	.buybtn .decrease{
		display: inline-block;
		padding: 6px;
		line-height: 24px;
		font-size: 24px;
		color: rgb(0,160,220);
	}
	.buybtn .count{
		display: inline-block;
		text-align: center;
		font-size: 10px;
		vertical-align: top;
		width: 12px;
		padding-top: 6px;
		line-height: 24px;
		color: rgb(147, 153, 159)
	}
	.buybtn .add{
		display: inline-block;
		padding: 6px;
		line-height: 24px;
		font-size: 24px;
		color: rgb(0,160,220);
	}
</style>