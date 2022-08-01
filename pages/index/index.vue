<template>
	<view>
		<button @click="ToImg">生成图片</button>
		<canvas style="width: 500px; height: 300px;" canvas-id="firstCanvas" id="firstCanvas"></canvas>
		<!-- <canvas style="width: 400px; height: 500px;" canvas-id="secondCanvas" id="secondCanvas"></canvas>
		<canvas style="width: 400px; height: 500px;" canvas-id="secondCanvas" id="secondCanvas" @error="canvasIdErrorCallback"></canvas> -->
	</view>
</template>
<script>
export default {
	data () {
	      return {
			url:[]
		}
	}, 
	created()  {
		var context = uni.createCanvasContext('firstCanvas')
			context.drawImage('http://pic.bizhi360.com/bbpic/51/10551.jpg')
							
		//背景填充
		// this.fillRoundRect(context, 0, 0, 500, 300, 0, 'rgba(44, 225, 12, 0.7)');
		//时间框绘制
		this.strokeRoundRect(context, 130, 10, 60,20, 5,1,"#8b8b8b");
		//投稿气泡绘制
		this.strokeRoundRect(context, 50, 50, 300,100, 5,1,"#8b8b8b");
		//小黑板气泡绘制
		this.strokeRoundRect(context, 300, 200, 50,50, 5,1,"#8b8b8b");
		context.draw();
	},
	methods: {
		ToImg(){
			var urls=[];
			uni.canvasToTempFilePath({
			  // x: 100,起始点
			  // y: 200,结束点
			 width:500,
			 height:300,
			 destWidth: 1000,
			   destHeight: 1000,
			  canvasId: 'firstCanvas',
			  success: function(res) {
			    // 在H5平台下，tempFilePath 为 base64
			    console.log(res.tempFilePath)
				urls.push(res.tempFilePath);
				uni.previewImage({
						current:0,
							urls: urls,
							longPressActions: {
								itemList: ['发送给朋友', '保存图片', '收藏'],
								success: function(data) {
									console.log('选中了第' + (data.tapIndex + 1) + '个按钮,第' + (data.index + 1) + '张图片');
								},
								fail: function(err) {
									console.log(err.errMsg);
								}
							},
							});
			  } 
			})
			
			
					// uni.saveImageToPhotosAlbum({
					// 	 filePath: this.url,
					// 	// filePath: res.tempFilePaths[0],
					// 	success: function () {
					// 		console.log('save success');
					// 	}
					// });
			
		},
		starp(){		
		//绘制并填充一个圆角矩形  
		this.fillRoundRect(context, 200, 10, 100, 50, 10, 'rgba(0,0,0,0.7)');
		},
		/**该方法用来绘制一个有填充色的圆角矩形 
		     *@param cxt:canvas的上下文环境 
		     *@param x:左上角x轴坐标 
		     *@param y:左上角y轴坐标 
		     *@param width:矩形的宽度 
		     *@param height:矩形的高度 
		     *@param radius:圆的半径 
		     *@param fillColor:填充颜色 
		     **/
		fillRoundRect(cxt, x, y, width, height, radius, /*optional*/ fillColor) {
		      //圆的直径必然要小于矩形的宽高          
		      if (2 * radius > width || 2 * radius > height) {
		        return false;
		      }
		
		      cxt.save();
		      cxt.translate(x, y);
		      //绘制圆角矩形的各个边  
		      this.drawRoundRectPath(cxt, width, height, radius);
		      cxt.fillStyle = fillColor || "#000"; //若是给定了值就用给定的值否则给予默认值  
		      cxt.fill();
		      cxt.restore();
		    },
		 /**该方法用来绘制圆角矩形 
		     *@param cxt:canvas的上下文环境 
		     *@param x:左上角x轴坐标 
		     *@param y:左上角y轴坐标 
		     *@param width:矩形的宽度 
		     *@param height:矩形的高度 
		     *@param radius:圆的半径 
		     *@param lineWidth:线条粗细 
		     *@param strokeColor:线条颜色 
		     **/
		strokeRoundRect(cxt, x, y, width, height, radius, /*optional*/ lineWidth, /*optional*/ strokeColor) {
		      //圆的直径必然要小于矩形的宽高          
		      if (2 * radius > width || 2 * radius > height) {
		        return false;
		      }
		
		      cxt.save();
		      cxt.translate(x, y);
		      //绘制圆角矩形的各个边  
		      this.drawRoundRectPath(cxt, width, height, radius);
		      cxt.lineWidth = lineWidth || 2; //若是给定了值就用给定的值否则给予默认值2  
		      cxt.strokeStyle = strokeColor || "#000";
		      cxt.stroke();
		      cxt.restore();
		    },
			
		drawRoundRectPath(cxt, width, height, radius) {
		  cxt.beginPath(0);
		  //从右下角顺时针绘制，弧度从0到1/2PI  
		  cxt.arc(width - radius, height - radius, radius, 0, Math.PI / 2);

		  //矩形下边线  
		  cxt.lineTo(radius, height);

		  //左下角圆弧，弧度从1/2PI到PI  
		  cxt.arc(radius, height - radius, radius, Math.PI / 2, Math.PI);

		  //矩形左边线  
		  cxt.lineTo(0, radius);

		  //左上角圆弧，弧度从PI到3/2PI  
		  cxt.arc(radius, radius, radius, Math.PI, Math.PI * 3 / 2);

		  //上边线  
		  cxt.lineTo(width - radius, 0);

		  //右上角圆弧  
		  cxt.arc(width - radius, radius, radius, Math.PI * 3 / 2, Math.PI * 2);

		  //右边线  
		  cxt.lineTo(width, height - radius);
		  cxt.closePath();
		  
		}
			  
	}
}
</script>

<style>
	.container {
		padding: 20px;
		font-size: 14px;
		line-height: 24px;
	}
</style>
