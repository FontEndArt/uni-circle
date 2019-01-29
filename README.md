# uni-app 进度环插件uni-app 进度环插件
------------

### 使用说明：
参考pages/xiaoran-circle/xiaoran-circle.vue引入组件
组件地址：components/xiaoran-circle/xiaoran-circle.vue

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
	}
},
```


## 引入示例:
```
··
···


	<Circle :percent="percent" size="200" :stroke-color="color">
		<icon v-if="percent == 100" type="success" size="60" color="#5cb85c" />
		<text v-else style="font-size:24px; color: #FFFFFF;">{{ percent }}%</text>
	</Circle>

	<Circle :percent="percent" size="200" :stroke-color="color" BgId="BgId" InId="InId">
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
	</Circle>

···

<script>
	import Circle from '@/pages/common/xiaoran-canvas-circle/xiaoran-circle.vue';
	export default {
		components: {
			Circle
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
```

## 预览图：
![preview](https://github.com/FontEndArt/uni-circle/blob/master/preview.jpg?raw=true "preview")
![单页面引入多个](https://github.com/FontEndArt/uni-circle/blob/master/preview.png?raw=true "单页面引入多个")

[uni-app插件市场](https://ext.dcloud.net.cn/ "uni-app插件市场")
