<template>
	<view
		:style="{
			width: circleSize.width,
			height: circleSize.height
		}"
		class="circle_box"
	>
		<canvas
			class="CanvasBox strokeCanvas"
			canvas-id="BgCanvas"
		></canvas>
		<canvas
			class="CanvasBox trailCanvas"
			canvas-id="InCanvas"
		></canvas>
		<view class="slot">
			<slot></slot>
		</view>
	</view>
</template>

<script>
	export default {
		name: "iCircle",
		props: {
			percent: {
				// 百分比
				type: Number,
				default: 0
			},
			size: {
				// 图表的宽度和高度，单位 upx
				type: Number,
				default: 120
			},
			strokeWidth: {
				// 进度环的线宽，单位 upx
				type: Number,
				default: 10
			},
			strokeColor: {
				// 进度环的颜色 16进制
				type: String,
				default: "#2d8cf0"
			},
			trailWidth: {
				// 进度环背景的线宽，单位 upx
				type: Number,
				default: 12
			},
			trailColor: {
				// 进度环背景的颜色
				type: String,
				default: "#eaeef2"
			},
			dashboard: {
				// 仪表盘待定
				type: Boolean,
				default: false
			}
		},
		data() {
			return {
				selfPercent: 0,
				type: "add"
			}
		},
		computed: {
			UniSize() {
				// 转换为upx
				return uni.upx2px(this.size);
			},
			circleSize() {
				return {
					width: `${this.UniSize}px`,
					height: `${this.UniSize}px`
				};
			},
			center_x() {
				return this.UniSize / 2;
			},
			center_y() {
				return this.UniSize / 2;
			},
			rad() {
				return Math.PI*2/100;
			},
			lineWidth() {
				return uni.upx2px(this.trailWidth);
			},
			BgWidth() {
				return uni.upx2px(this.strokeWidth);
			},
			radius() {
				return this.center_x - this.BgWidth - Math.abs(this.BgWidth - this.lineWidth);
			}
		},
		methods: {
			Init() {
				this.backgroundCircle();
				this.requestAnimationFrame();
			},
			requestAnimationFrame() {
				if (this.type == "add") {
					if (this.selfPercent > this.percent) {
						return;
					}
					this.selfPercent++;
					if (this.selfPercent >= 100) {
						this.selfPercent = 100;
					}
				} else {
					if (this.selfPercent < this.percent) {
						return;
					}
					this.selfPercent--;
					if (this.selfPercent <= 0) {
						this.selfPercent = 0;
					}
				}
				this.foregroundCircle();
				setTimeout(this.requestAnimationFrame, 5);
			},
			backgroundCircle() {
				const context = uni.createCanvasContext('BgCanvas');
				// 绘制背景圆环
				const rad = Math.PI*2/100;

				context.save();
				// 开始路径绘制
				context.beginPath();
				context.setLineWidth(this.lineWidth); //设置线宽
				context.setLineCap("round");
				context.setStrokeStyle(this.trailColor);
				//用于绘制圆弧context.arc(x坐标，y坐标，半径，起始角度，终止角度，顺时针/逆时针)
				context.arc(this.center_x, this.center_y, this.radius, 0, Math.PI * 2, false);
				context.stroke();
				// 结束绘制路径
				context.closePath();
				// 恢复之前保存的绘图上下文。
				context.restore();
				// 绘制
				context.draw()
			},
			foregroundCircle(){
				const context = uni.createCanvasContext('InCanvas');
                context.save();
                context.setStrokeStyle(this.strokeColor);
				context.setLineWidth(this.BgWidth); //设置线宽
				context.setLineCap("round");
                context.beginPath();
                context.arc(this.center_x, this.center_y, this.radius , -Math.PI/2, -Math.PI/2 + this.selfPercent * this.rad, false);
                context.stroke();
                context.closePath();
                context.restore();
				context.draw();
            }
		},
		mounted() {
			this.Init();
		},
		onReady() {
		},
		watch: {
			percent(newV, oldV) {
				console.log(newV);
				console.log(oldV);
				if (newV > oldV) {
					this.type = "add";
				} else {
					this.type = "reduce";
				}
				this.Init();
			}
		}
	};
</script>

<style>
	@import url("./xiaoran-circle.css");
</style>
