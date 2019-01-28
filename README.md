# uni-app 进度环插件uni-app 进度环插件
------------

### 使用说明：
参考pages/xiaoran-circle/xiaoran-circle.vue引入组件
组件地址：components/xiaoran-circle/xiaoran-circle.vue

## props说明:
```
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
		}
	}
```


## 引入示例:
```
··
···

<Circle :percent="percent" size="200" :stroke-color="color">
	<icon v-if="percent == 100" type="success" size="60" color="#5cb85c" />
	<text v-else style="font-size:24px; color: #FFFFFF;">{{ percent }}%</text>
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

[uni-app插件市场](https://ext.dcloud.net.cn/ "uni-app插件市场")


