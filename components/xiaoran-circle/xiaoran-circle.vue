<template>
	<view
		:style="{
			width: circleSize.width,
			height: circleSize.height
		}"
		class="circle_box"
	>
		<view class="canvas">
			<slot name="canvas"></slot>
		</view>
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
			prefix: {
				// 多个圆环情况下的前缀
				type: String,
				default: ""
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
			BgId: {
				// BgId
				type: String,
				required: true
			},
			InId: {
				// InId
				type: String,
				required: true
			},
			dashboard: {
				// 仪表盘
				type: Boolean,
				default: false
			},
			start: {
				// 仪表盘起始角度
				type: Number,
				default: 27
			}
		},
		data() {
			return {
				selfPercent: 0,
				type: "add",
				T: null
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
			},
			startDeg() {
				return Math.PI * (1 - this.start/180);
			},
			countDeg() {
				return this.start * 2 + 180;
			},
			fillDegArr() {
				let start, end;
				if (this.dashboard) {
					start = this.startDeg;
					end = Math.PI*this.selfPercent/100*this.countDeg/180 + this.startDeg;
				} else {
					start = -Math.PI/2;
					end = -Math.PI/2 + this.selfPercent * this.rad;
				}
				return {start, end};
			},
			bgDegArr() {
				let start, end;
				if (this.dashboard) {
					start = this.startDeg;
					end = Math.PI*this.countDeg/180 + this.startDeg;
				} else {
					start = 0;
					end = Math.PI * 2;
				}
				return {start, end};
			}
		},
		methods: {
			Init() {
				this.backgroundCircle();
				this.requestAnimationFrame();
			},
			requestAnimationFrame() {
				if (this.type == "add") {
					if (this.selfPercent >= this.percent) {
						clearTimeout(this.T);
						return;
					}
					this.selfPercent++;
					if (this.selfPercent >= 100) {
						this.selfPercent = 100;
					}
				} else {
					if (this.selfPercent <= this.percent) {
						clearTimeout(this.T);
						return;
					}
					this.selfPercent--;
					if (this.selfPercent <= 0) {
						this.selfPercent = 0;
					}
				}
				this.foregroundCircle();
				this.T = setTimeout(this.requestAnimationFrame, 5);
			},
			backgroundCircle() {
				const context = uni.createCanvasContext(this.BgId);
				// 绘制背景圆环
				const rad = Math.PI*2/100;

				context.save();
				// 开始路径绘制
				context.beginPath();
				context.setLineWidth(this.lineWidth); //设置线宽
				context.setLineCap("round");
				context.setStrokeStyle(this.trailColor);
				//用于绘制圆弧context.arc(x坐标，y坐标，半径，起始角度，终止角度，顺时针/逆时针)
				// context.arc(this.center_x, this.center_y, this.radius, 0, Math.PI * 2, false);
				context.arc(this.center_x, this.center_y, this.radius , this.bgDegArr.start, this.bgDegArr.end, false);
				context.stroke();
				// 结束绘制路径
				context.closePath();
				// 恢复之前保存的绘图上下文。
				context.restore();
				// 绘制
				context.draw()
			},
			foregroundCircle(){
				const context = uni.createCanvasContext(this.InId);
                context.save();
                context.setStrokeStyle(this.strokeColor);
				context.setLineWidth(this.BgWidth); //设置线宽
				context.setLineCap("round");
                context.beginPath();
				context.arc(this.center_x, this.center_y, this.radius , this.fillDegArr.start, this.fillDegArr.end, false);
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
