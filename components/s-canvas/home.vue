<template>
	<view class="">
		<view class="hideCanvasView">
			<canvas class="hideCanvas" :canvas-id="canvasId"  :style="'width:'+bgWidth+'px;height:'+bgHeight+'px;'"></canvas>
		</view>
		
		<button @tap.stop="getCanvas">生成海报</button>
	</view>
</template>

<script>
	let context;
	export default {
		props:{
			canvasId:{
				type:String,
				default:'',
			},
			canvasData:{
				type:Array,
				default:[],
			},
			bgImg:{
				type:String,
				default:'',
			}
		},
		data:function(){
			return {
				bgWidth:'',
				bgHeight:'',
			}
		},
		methods:{
			getCanvas:function(){
				let that=this;
				if(!that.canvasId){
					console.log("canvas-id 不能为null")
					return;
				}
				if(!that.canvasData){
					console.log("至少要有一条数据")
					return;
				}
				
				context=uni.createCanvasContext(that.canvasId,(that || null))
				
				that.startCanvas(that.canvasData)
			},
			startCanvas: async function(canvasData){
				let that=this;
				console.log("startCanvas")
				uni.showLoading({
					title:'图片加载中'
				})
				if(that.bgImg){
					await that.getbgImgInfo(that.bgImg)
					await that.drawBgImg(that.bgImg)
				}
				for(let item of canvasData){
					
					switch(item.type){
						case 'fillRect':
						await that.setFillRect(item)
						break;
						case 'fillText':
						await that.setFillText(item)
						break;
						case 'drawImage':
						// item = await that.getImgInfo(item)
						if(item.isCircle){
							await that.setImgCircle(item)
						}else{
							await that.setImgrect(item)
						}
						// await that.setDrawImage(item)
						break;
					}
				}
				uni.hideLoading()
				setTimeout(()=>{
					context.draw(true,()=>{
						uni.canvasToTempFilePath({
							canvasId:that.canvasId,
							
							success:function(res){
								console.log("看图")
								console.log(res.tempFilePath)
								that.$emit('getImgSrc',res.tempFilePath)
							}
						},that)
					})
				},100)
				
			},
			drawBgImg:function(bgImg){
				let that=this;
				return new Promise((res,rej)=>{
					context.save()
					context.drawImage(bgImg)
					context.restore()
					context.draw(true)
					res('success')
				})
			},
			getbgImgInfo:function(bgImg){
				let that=this;
				return new Promise((res,rej)=>{
					uni.getImageInfo({
						src:bgImg,
						success:function(image){
							console.log(image)
							that.bgWidth=image.width
							that.bgHeight=image.height
							res('success')
						}
					})
				})
			},
			getImgInfo:function(drawImage){
				return new Promise((res,rej)=>{
					uni.getImageInfo({
						src:drawImage.imageResource,
						fileType:'jpg',
						success:function(image){
							drawImage.imageResource=image.path
						//	console.log(image)
							res(drawImage)
						}
					})
					
				})
			},
			setFillRect: function(fillRect){
				return new Promise((res,rej)=>{
					console.log("画框")
					console.log(fillRect)
					
					context.save()
					context.beginPath()
					context.setFillStyle(fillRect.style)
					context.setGlobalAlpha(fillRect.alpha)
					context.fillRect(fillRect.x,fillRect.y,fillRect.width,fillRect.height)
					context.restore()
					
					context.draw(true)
					
					
					res('success')
				})
				
			},
			setFillText:  function(fillText){
				return new Promise((res,rej)=>{
					console.log('画字')
					console.log(fillText)
				
					context.save()
					context.beginPath()
					context.setGlobalAlpha(fillText.alpha)
					context.setFillStyle(fillText.style)
				//	context.setLineWidth(50)
					
					context.setFontSize(fillText.fontSize)
					context.fillText(fillText.text,fillText.x,fillText.y,fillText.maxWidth)
					context.restore()
					context.draw(true)
				
					res("success")
				})
				
			},
			setImgrect:function(drawImage){
				return new Promise((res,rej)=>{
					if(drawImage){
					console.log('画图')
					console.log(drawImage)
					
					context.save()
					context.beginPath()
					context.setGlobalAlpha(drawImage.alpha)
					context.drawImage(drawImage.imageResource,drawImage.dx,drawImage.dy,drawImage.dWidth,drawImage.dHeight)
					//context.stroke()
					context.restore()
					context.draw(true)
					res("success")
					
					}else{
						rej("full")
					}
				})
			},
			setImgCircle:function(drawImage){
				return new Promise((res,rej)=>{
					if(drawImage){
					console.log('画图')
					console.log(drawImage)
					var avatarurl_width = drawImage.dWidth;    //绘制的头像宽度
					var avatarurl_heigth = drawImage.dHeight;   //绘制的头像高度
					var avatarurl_x = drawImage.dx;   //绘制的头像在画布上的位置
					var avatarurl_y = drawImage.dy;   //绘制的头像在画布上的位置
					context.save()
					context.beginPath()
					context.setGlobalAlpha(drawImage.alpha)
					context.arc(avatarurl_width / 2 + avatarurl_x, avatarurl_heigth / 2 + avatarurl_y, avatarurl_width / 2, 0, Math.PI * 2, false);
					context.clip()
					context.drawImage(drawImage.imageResource,avatarurl_x, avatarurl_y, avatarurl_width, avatarurl_heigth)
					//context.stroke()
					context.restore()
					context.draw(true)
					res("success")
					
					}else{
						rej("full")
					}
				})
			},
			
			
		}
	}
</script>

<style>
	.myCanvas{
	
	}
	.hideCanvasView {
		position: relative;
	}
	
	.hideCanvas {
		position: fixed;
		top: -99999upx;
		left: -99999upx;
		z-index: -99999;
	}
</style>
