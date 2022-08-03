<template>
	<view>
		<button @click="BeginDraw">绘制图片</button>
		<button @click="ToImg">生成图片</button>
		<canvas :style="[firstCanvas]" canvas-id="firstCanvas" ref="firstCanvas" id="firstCanvas"></canvas>
		<!-- <canvas style="width: 400px; height: 500px;" canvas-id="secondCanvas" id="secondCanvas"></canvas>
		<canvas style="width: 400px; height: 500px;" canvas-id="secondCanvas" id="secondCanvas" @error="canvasIdErrorCallback"></canvas> -->
	</view>
</template>
<script>
export default {
	data () {
	      return {
			  //画布实际尺寸
			  firstCanvas:{
			  'width': '500px',
			  'minHeight': '500px'
			},
			backgroundData:{
				frameTopLeftX:0,
				frameTopLeftY:0,
				frameWidth:500,
				frameHeight:280,
				xhbImgSrc:"../../static/bjt.jpg"
			},
				//时间
				timeData:{
					frameTopLeftX:195,
					frameTopLeftY:42,
					frameWidth:110,
					frameHeight:20,
					contentTopLeftX:203, 
					contentTopLeftY:58,
					contentTxt:'2022-8-2 11:43',
					fontColor:"",
					radius:5 ,//圆角
					fontSizeAndStyle:"13px Microsoft Yahei",
					fillColor:'rgb(215,215,215)'
				},
				//投稿
				submitData:{
					frameTopLeftX:20,
					frameTopLeftY:105,
					frameWidth:389,
					frameHeight:'',
					computeDrawHeight:'',
					contentTopLeftX:33,
					contentTopLeftY:117,
					txt:'',
					fontSizeAndStyle:"20px Microsoft Yahei",
					result:{},
					radius:10,//圆角
					fontColor:"#333333",
					fillColor:'rgba(230,230,230,0.7)'
				},
				//类型
				categoryData:{
					frameTopLeftX:'20',
					frameTopLeftY:'80',
					frameWidth:'50',
					frameHeight:'20',
					contentTopLeftX:'30',
					contentTopLeftY:'95',
					contentTxt:'提问',
					radius:5 ,//圆角
					fontSizeAndStyle:"15px Microsoft Yahei",
					fontColor:"#FFFFFF",
					fillColor:'rgb(112,182,3)'
					
				},
				//回复气泡
				respondData:{
					frameTopLeftX:'',
					frameTopLeftY:'',
					frameWidth:'',
					frameHeight:'',
					contentTopLeftX:'',
					contentTopLeftY:'',
				},
				headshotData:{
					frameCenterX:'',
					frameCenterY:'',
					frameR:'',
					xhbUrl:'',
					bbbUrl:''
				}

			
		}
	}, 
	onReady() {
		//var context = uni.createCanvasContext('firstCanvas')
	},
	created()  {
		this.submitData.txt="小黑板你好小黑板你好小黑板你好小黑板你好小黑板你好小黑板你好小黑板小黑板你好小黑板你好小黑板你好小黑板你好小黑板你好小小黑板你好小黑板你好小黑板你好小黑板你好小黑板你好小小黑板你好小黑板你好小黑板你好小黑板你好小黑板你好小小黑板你好小黑板你好小黑板你好小黑板你好小黑板你好小你好小黑板你好";
		//var txt="小黑板你好";
		this.submitData.result = this.text(this.submitData.txt)
		this.submitData.DrawHeight=500-30+this.submitData.result.rows*30
		this.firstCanvas.minHeight=this.submitData.DrawHeight+"px"
		// var context = uni.createCanvasContext('firstCanvas')
		// uni.getImageInfo({
		// 	src:"https://s1.328888.xyz/2022/08/02/Oht1K.jpg",
		// 	success: (image) => {
		// 		this.popupImg=image.path;
		// 		console.log("获取到的本地路径",image)
		// 	}
		// })
		// context.drawImage(this.popupImg,0,0,500,280);
		// //context.drawImage(imgBase64,0,0,500,280);						
		// //背景填充
		//  //this.fillRoundRect(context, 0, 0, 500, 500, 0, 'rgba(93, 93, 93, 0.7)');
		// //时间框绘制
		// this.strokeRoundRect(context, 195, 42, 110,20, 5,1,"#8b0002");
		// //绘制时间
		// // context.font="Microsoft Yahei";
		//  context.font="13px Microsoft Yahei";
		// context.fillText("2022-8-1 10:00",200,58);
		
		// //类型绘制
		//  context.font="15px Microsoft Yahei";
		// this.strokeRoundRect(context,20,80,50,20,5,1,"#8b0002");
		// //绘制类别文字
		// context.fillText("提问",30,95);
		
		// //投稿内容
		// var txt="小黑板你好小黑板你好小黑板你好小黑板你好小黑板你好小黑板你好小黑板你好小黑板你好";
		// //var txt="小黑板你好";
		// let result = this.text(txt)
		// //投稿气泡绘制
		// //如果文字占一行，则根据文字长短调整气泡x轴
		// let resWitch=389;
		// if(result.rows==1){
		// 	resWitch=result.strLength*result.fontSize+30;
			
		// }
		// //根据文字行数确定气泡的高 35为单行高度
		// this.strokeRoundRect(context, 20, 105, resWitch,result.rows*35, 10,1,"#8b0002");
		// //绘制投稿内容
		//  context.font="18px Microsoft Yahei";
		// for (let i = 0; i <= result.rows; i++) {
		// //35为文字x方向起始位置，115为文字y轴起始方向，23为每增加一行，y轴下移23px
		//  context.fillText(txt.slice(result.rowFontNum*(i-1),result.rowFontNum*i),35,110+i*23)
		// }
		// context.save();
		// //小黑板气泡绘制
		// this.strokeRoundRect(context, 322, 301, 100,50, 10,1,"#8b0002");
		// context.fillText("板板收到~",325,330);
		// //小黑板头像绘制
		// this.drawCircle(context,463,302,25,-(1 / 2 * Math.PI),-(1 / 2 * Math.PI),1);
		// context.draw();
	},
	methods: {
		BeginDraw(){
			// uni.showLoading({
			// 	title: '加载中'
			// });
			var context = uni.createCanvasContext('firstCanvas')
			// uni.getImageInfo({
			// 	src:"https://s1.328888.xyz/2022/08/02/Oht1K.jpg",
			// 	success: (image) => {
			// 		pathToBase64(image.path)
			// 								.then(base64 => {
			// 									this.popupImgBase64 = base64
			// 								})
			// 								.catch(error => {
			// 								console.error(error)
			// 								})
										
			// 		console.log("获取到的本地路径",image)
			// 		console.log("base64",this.popupImgBase64)
			// 	}
			// })
			
			//根据画布高度遍历背景图片
			let tempBgH=this.backgroundData.frameTopLeftY;
			for (var i = 0; i < Math.ceil(parseInt(this.firstCanvas.minHeight)/this.backgroundData.frameHeight); i++) {
				context.drawImage(this.backgroundData.xhbImgSrc,this.backgroundData.frameTopLeftX,tempBgH,this.backgroundData.frameWidth,this.backgroundData.frameHeight);
				context.save();
				tempBgH+=this.backgroundData.frameHeight;
			}
			
			//时间框绘制
					this.fillRoundRect(context, this.timeData.frameTopLeftX, this.timeData.frameTopLeftY, this.timeData.frameWidth,this.timeData.frameHeight, this.timeData.radius,this.timeData.fillColor);
					//绘制时间
					 context.font=this.timeData.fontSizeAndStyle;
					context.fillText(this.timeData.contentTxt,this.timeData.contentTopLeftX,this.timeData.contentTopLeftY);
					
					//类型绘制
					 context.font=this.categoryData.fontSizeAndStyle;
					this.fillRoundRect(context,this.categoryData.frameTopLeftX,this.categoryData.frameTopLeftY,this.categoryData.frameWidth,this.categoryData.frameHeight,this.categoryData.radius,this.categoryData.fillColor);
					//绘制类别文字
					context.fillStyle=this.categoryData.fontColor;
					context.fillText(this.categoryData.contentTxt,this.categoryData.contentTopLeftX,this.categoryData.contentTopLeftY);
					
					
					//投稿绘制
					context.font="20px Microsoft Yahei";
					//投稿内容
					
					//投稿气泡绘制
					//如果文字占一行，则根据文字长短调整气泡x轴
					let resWitch=this.submitData.frameWidth;
					if(this.submitData.result.rows==1){
						resWitch=this.submitData.result.strLength*this.submitData.result.fontSize+30;
						
					}
					//根据文字行数确定气泡的高 35为单行高度
					
					this.fillRoundRect(context, this.submitData.frameTopLeftX, this.submitData.frameTopLeftY, resWitch,this.submitData.result.rows*30+25, this.submitData.radius,this.submitData.fillColor);
					//绘制投稿内容
					 context.font=this.submitData.fontSizeAndStyle;
					 context.fillStyle=this.submitData.fontColor;
					for (let i = 0; i <= this.submitData.result.rows; i++) {
					//35为文字x方向起始位置，115为文字y轴起始方向，23为每增加一行，y轴下移23px
					 context.fillText(this.submitData.txt.slice(this.submitData.result.rowFontNum*(i-1),this.submitData.result.rowFontNum*i),this.submitData.contentTopLeftX,this.submitData.contentTopLeftY+i*26)
					}
					context.save();
					//小黑板气泡绘制
					this.fillRoundRect(context, 322, 270+this.submitData.result.rows*30, 110,60, 10,'rgb(0,153,255)');
					context.fillStyle="#FFFFFF";
					context.fillText("板板收到~",327,305+this.submitData.result.rows*30);
					//小黑板头像绘制
					this.drawCircle(context,463,302+this.submitData.result.rows*30,25,-(1 / 2 * Math.PI),-(1 / 2 * Math.PI),1);
					context.draw();
					
			setTimeout(function () {
				
				// uni.hideLoading();
				
				//context.draw();
			}, 5000);
		},
		//canvas绘制文字换行
		text(str) {
		      // 画布总宽度 px单位
		      let canvasWidth = 360
		      // 字体大小 px单位
		      let fontSize = 20
		      // 每行所需字数 = 画布总宽度 / 单个字体大小
		      let rowFontNum = Math.floor(canvasWidth / fontSize)
		      // 字符串总长度
		      let strLength = str.length
		      // 所需行数 = 字符总长度 / 每行所需字数
		      let rows = Math.ceil(strLength / rowFontNum)
		      return {canvasWidth,fontSize,rowFontNum,strLength,rows}
		  },
		ToImg(){
			var urls=[];
			uni.canvasToTempFilePath({
			  // x: 100,起始点
			  // y: 200,结束点
			 width:500,
			 height:this.DrawHeight,
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
			
		},
		drawCircle(cxt,x,y,cR,startAngle,tmpAngle,lineWidth){
			 cxt.beginPath();
			    cxt.lineWidth = lineWidth;
			    cxt.strokeStyle = '#8b0002';
			    cxt.arc(x, y, cR, startAngle, tmpAngle);
			    cxt.stroke();
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
