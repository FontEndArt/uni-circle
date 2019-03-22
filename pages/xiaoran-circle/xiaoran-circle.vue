<template>
	<view class="box">
		<view class="view_box">
			<iCircle
				:percent="percent"
				:size="300"
				:stroke-color="color"
				:stroke-width="20"
				:trail-width="20"
				:dashboard="true"
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
		</view>
		<view class="button_box">
			<button type="primary" class="button_item" @click="add">+</button>
			<button type="primary" class="button_item" @click="minus">-</button>
		</view>
	</view>
</template>

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
</script>

<style>
	.box {
		/* margin-top: var(--status-bar-height); */
		flex: 1;
	}
	.view_box {
		width: 100%;
		background: #000000;
		display: flex;
		align-items: center;		justify-content: center;
	}
	.button_box {
		display: flex;
		flex-direction: column;
		align-items: stretch;
		justify-content: flex-start;
		padding: 0 30upx;
	}
	.button_item {
		width: 100%;
		margin-top: 30upx;
	}
	
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
</style>
