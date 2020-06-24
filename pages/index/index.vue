<template>
	<view class="content">
		
		
		
		<sCanvas :canvasId="'myCanvas'" :canvasData="canvasData" :bgImg="bgImg" @getImgSrc="getImgSrc"></sCanvas>
		
		<view class="" v-if="url">
			<image :src="url" mode="widthFix"></image>
		</view>
		
		
		
	</view>
</template>

<script>
	import sCanvas from '../../components/s-canvas/home.vue'
	export default {
		data() {
			return {
				url:'',
				bgImg:'../../static/bgImg.jpg',
				canvasData:[
					{
					type:'fillRect',
					x:0,
					y:1470,
					width:1080,
					height:450,
					alpha:0.3,
					style:'#EEEEEE',
					},
					{
					type:'fillText',
					x:50,
					y:1520,
					text:'少帅大是大非旷视科技逢狼时刻解放路斯科拉里看见',
					maxWidth:1000,
					fontSize: 50,
					
					alpha:1,
					style:'red',	
					},
					{
						type:'drawImage',
						isCircle:false,
						
						alpha:1,
						imageResource:'../../static/logo.png',
						
						dx:800,
						dy:1600,
						dWidth:200,
						dHeight:200,
						
					},
					{
					type:'fillText',
					x:260,
					y:1690,
					text:'少帅大是大非旷视科技逢狼时刻解放路斯科拉里看见',
					maxWidth:500,
					fontSize: 50,
					
					alpha:1,
					style:'red',	
					},
					{
					type:'fillText',
					x:260,
					y:1750,
					text:'少帅大是大非旷视科技逢狼时刻解放路斯科拉里看见',
					maxWidth:500,
					fontSize: 40,
					
					alpha:1,
					style:'red',	
					},
					{
						type:'drawImage',
						isCircle:true,
						
						alpha:1,
						imageResource:'../../static/logo.png',
						
						dx:50,
						dy:1600,
						dWidth:200,
						dHeight:200,
						
					}
				],
			}
		},
		components:{
			sCanvas
		},
		onLoad() {

		},
		methods: {
			getImgSrc:function(src){
				this.url=src
			},	
			getCanvas:function(){
			
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
