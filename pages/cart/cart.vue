<template>
	<view>
		<my-address></my-address>
		<view class="container" v-if="cart.length !== 0">
		<view class="title">
			<uni-icons type="shop" size="18"></uni-icons>
			<text class="title-text">购物车</text>
		</view>
		
		<uni-swipe-action>
			<block v-for="(goods,i) in cart" :key='i'>
				<uni-swipe-action-item :right-options='options' @click='swipeActionClick(goods)'>
					<my-goods 
					:goods="goods" 
					:show-radio="true"
					@radio-change="radioChange" 
					:show-num="true"
					@num-change="numberChange"></my-goods>
				</uni-swipe-action-item>
			</block>
		</uni-swipe-action>
		<my-settle></my-settle>
	</view>
	<view class="empty" v-else>
		<text class="empty-text">sffgsdfgs555dfg</text>
	</view>
		
	</view>
	</view>
</template>

<script>
	import { mapMutations } from 'vuex'
	import badgeMix from '@/mixins/tabbar-badge.js'
	import { mapGetters } from 'vuex'
	import { mapState } from 'vuex'
	export default {
		 mixins: [badgeMix],
		data() {
			return {
				options:[{
					text:'删除',
					style:{
						 backgroundColor: '#c8cac9'
					}
				}]
			}
		},
		computed:{
			 ...mapGetters('m_cart', ['total']),
			  ...mapState('m_cart', ['cart']),
		},
		methods: {
			...mapMutations('m_cart', ['updateGoodsState', 'updateGoodsCount', 'removeGoodsById']),
			setBadge(){
				uni.setTabBarBadge({
					index:2,
					text:this.total+''
				})
			},
			swipeActionClick(goods){
				this.removeGoodsById(goods.goods_id)
			},
			radioChange(e){
				console.log(e)
				this.updateGoodsState(e)
			},
			numberChange(e){
				this.updateGoodsCount(e)
			}
		},
		onShow() {
			this.setBadge()
		},
		
	}
</script>

<style>
	.container{
		padding-bottom: 100rpx;
	}
	.title{
		height: 80rpx;
		display: flex;
		align-items: center;
		font-size: 28rpx;
		padding: 10rpx;
		border-bottom: 20rpx;
		
	}
	.title-text{
		margin-left: 20rpx;
	}
	.empty{
		 display: flex;
		  flex-direction: column;
		  align-items: center;
		  padding-top: 150px;
		  
	}
	.empty-text{
		font-size: 12px;
		    color: gray;
		    margin-top: 15px;	  
	}

</style>
