<template>
	<view v-show="ifshow" @tap="ableClose" @touchmove.stop.prevent class="popup-layer">
		<view ref="popRef" class="popup-content" @tap.stop="stopEvent" :style="_location">
			<slot></slot>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'popup-layer',
		props: {
			direction: {
				type: String,
				default: 'top', // 方向  top，bottom，left，right 
			},
			autoClose: {
				type: Boolean,
				default: true,
			},
			isTransNav: {
				type: Boolean,
				default: false
			},
			navHeight: {
				type: Number,
				default: 0
			},
			tabHeight: {
				type: Number,
				default: 0
			}
		},
		data() {
			return {
				ifshow: false, // 是否展示,
				translateValue: -100, // 位移距离
				timer: null,
				iftoggle: false,
			};
		},
		computed: {
			_translate() {
				const transformObj = {
					'top': `transform:translateY(${-this.translateValue}%)`,
					// 'bottom': `transform:translateY(calc(${this.translateValue}% + ${this.tabHeight}px))`,
					'bottom': `transform:translateY(${this.translateValue}%)`,
					'left': `transform:translateX(${-this.translateValue}%)`,
					'right': `transform:translateX(${this.translateValue}%)`
				};
				return transformObj[this.direction]
			},
			_location() {
				const positionValue = {
					'top': 'bottom:0px;width:100%;',
					'bottom': 'top:${this.tabHeight}px;width:100%;',
					'left': 'right:0px;height:100%;',
					'right': 'left:0px;height:100%;',
				};
				return positionValue[this.direction] + this._translate;
			}
		},
		methods: {
			show() {
				let _this = this;
				this.ifshow = true;
				let _open = setTimeout(() => {
					if (_this.isTransNav) {
						this.translateValue = this.navHeight;
					} else {
						this.translateValue = 0;
					}

					_open = null;
				}, 100)
				let _toggle = setTimeout(() => {
					this.iftoggle = true;
					_toggle = null;
				}, 300);
			},
			close() {
				if (this.timer !== null || !this.iftoggle) {
					return;
				}
				this.translateValue = -100 - this.navHeight - this.tabHeight;

				this.timer = setTimeout(() => {
					this.ifshow = false;
					this.timer = null;
					this.iftoggle = false;
				}, 300);
				this.$emit("close")
			},
			ableClose() {
				if (this.autoClose) {
					this.close();
				}
			},
			stopEvent(event) {},
		}
	}
</script>

<style>
	.popup-layer {
		position: fixed;
		z-index: 999999;
		background: rgba(0, 0, 0, .3);
		height: 100%;
		width: 100%;
		left: 0px;
	}

	.popup-content {
		position: fixed;
		z-index: 1000000;
		background: #FFFFFF;
		transition: all .3s ease;
	}
</style>
