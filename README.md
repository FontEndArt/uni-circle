# uni-app 进度环插件uni-app 进度环插件
------------

### 使用说明：
参考pages/xiaoran-circle/xiaoran-circle.vue引入组件
组件地址：components/xiaoran-circle/xiaoran-circle.vue

##下一版本待定
```
下一版本打算支持刻度线或者增加圆弧的空白间隔
```

##v1.3更新说明
```
1.修复标签冲突报错
2.修复部分样式错乱
```

##v1.2更新说明
```
1.修复template语法
2.调整圆形的绘制方法
3.增加不封闭的进度环参数
```

##v1.1更新说明
```
1.经测试官方canvas组件不支持<canvas :canvas-id="cid"></canvas>方式
2.曲向修复一个页面引用多个进度环，因canvasid一致导致的所有canvas都被隐藏
3.增加引入BgId、InId这两个props 暂时用来处理同一个页面引入多个进度环的情况
```

## props说明:
```javascript
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
		// BgId背景圆环CanvasID
		type: String,
		default: "BgCanvas"
	},
	InId: {
		// IgId进度圆环CanvasID
		type: String,
		default: "InCanvas"
	},
	dashboard: {
		// 仪表盘
		type: Boolean,
		default: false
	},
	start: {
		// 仪表盘起始角度 -> 和x轴的夹角
		type: Number,
		default: 27
	}
},
```


## 引入示例:
```
··
···

			<iCircle
				:percent="percent"
				size="300"
				:stroke-color="color"
				stroke-width="20"
				trail-width="20"
				dashboard="true"
			>
				<icon v-if="percent == 100" type="success" size="60" color="#5cb85c" />
				<text v-else style="font-size:24px; color: #FFFFFF;">{{ percent }}%</text>
			</iCircle>
			<iCircle
				:percent="percent"
				:size="200"
				:stroke-color="color"
				BgId="BgId"
				InId="InId"
			>
				<icon v-if="percent == 100" type="success" size="60" color="#5cb85c" />
				<text v-else style="font-size:24px; color: #FFFFFF;">{{ percent }}%</text>
				<view slot="canvas">
					<canvas
						class="CanvasBox strokeCanvas"
						canvas-id="BgId"
					></canvas>
					<canvas
						class="CanvasBox trailCanvas"
						canvas-id="InId"
					></canvas>
				</view>
			</iCircle>

···

<script>
	import iCircle from '@/components/xiaoran-circle/xiaoran-circle.vue';
	export default {
		components: {
			iCircle
		},
		data() {
			return {
				percent: 50
			}
		},
		computed: {
			color() {
				let color = '#2db7f5';
				if (this.percent == 100) {
					color = '#5cb85c';
				}
				return color;
			}
		},
		methods: {
			add() {
				if (this.percent >= 100) {
					return false;
				}
				this.percent += 10;
			},
			minus() {
				if (this.percent <= 0) {
					return false;
				}
				this.percent -= 10;
			}
		}
	}
</scirpt>

<style>
···
.CanvasBox {
	width: 100%;
	height: 100%;
	position: absolute;
	top: 0px;
	left: 0px;
	
	display: flex;
	justify-content: center;
	align-items: center;
}
···
</style>
```

## 预览图：
![预览图](https://github.com/FontEndArt/uni-circle/blob/master/preview.jpg?raw=true "预览图")

[uni-app插件市场](https://ext.dcloud.net.cn/ "uni-app插件市场")

