<template>
	<div class="HorizontalThreeWindowContent" ref="content">
		<div class="left" draggable="false" :style="{'width':leftWidth+'px',flexShrink:0}" ref="left">
			<slot name="left"></slot>
		</div>
		<div class="divide" draggable="false" @mousedown="leftDivideMouseDown($event)" :style="{'width':ply+'px','left':(leftWidth-(ply/2))+'px'}">
			<div class="divideDisplay" :style="{'width':plyDisplay+'px'}"></div>
		</div>
		<div class="center" draggable="false" ref="center">
			<slot name="center"></slot>
		</div>
		<div class="divide" draggable="false" @mousedown="rightDivideMouseDown($event)" :style="{'width':ply+'px','right':(rightWidth-(ply/2))+'px'}">
			<div class="divideDisplay" :style="{'width':plyDisplay+'px'}"></div>
		</div>
		<div class="right" draggable="false" :style="{'width':rightWidth+'px'}" ref="right">
			<slot name="right"></slot>
		</div>
	</div>
</template>

<script>
	export default {
		name: "HorizontalThreeWindow",
		props: {
			ply: {
				type: Number,
				default: 6
			}, //隔断厚度
			plyDisplay: {
				type: Number,
				default: 2
			}, //隔断的视觉厚度
			leftWidth: {
				type: Number,
				default: 200
			},
			rightWidth: {
				type: Number,
				default: 200
			}
		},
		methods: {
			leftDivideMouseDown(event) {
				let content = this.$refs['content'];
				//计算前数据
				let position = event.clientX;
				let left = this.$refs['left'];
				let leftDivide = event.currentTarget; //绑定事件的元素，target是触发的元素。
				let width = left.offsetWidth;
				let leftDivideLeft = parseInt(leftDivide.style.left);

				function onMove(e) {
					let nowPosition = e.clientX;
					let d = nowPosition - position;
					left.style.width = (width + d) + 'px';
					leftDivide.style.left = (leftDivideLeft + d) + 'px';
				}

				function onUp() {
					content.removeEventListener("mousemove", onMove);
					content.removeEventListener('mouseup', onUp);
				}
				content.addEventListener('mousemove', onMove, false);
				content.addEventListener('mouseup', onUp, false);
			},
			rightDivideMouseDown(event) {
				let content = this.$refs['content'];
				//计算前数据
				let position = event.clientX;
				let right = this.$refs['right'];
				let rightDivide = event.currentTarget; //绑定事件的元素，target是触发的元素。
				let width = right.offsetWidth;
				let rightDivideRight = parseInt(rightDivide.style.right);

				function onMove(e) {
					let nowPosition = e.clientX;
					let d = -(nowPosition - position);
					right.style.width = (width + d) + 'px';
					rightDivide.style.right = (rightDivideRight + d) + 'px';
					//leftDiveide的位置永远等于左边的元素的宽度，所以就不用记录之前的位置了，这就是用了一个小小的等式换算，可以节省资源。
				}

				function onUp() {
					content.removeEventListener("mousemove", onMove);
					content.removeEventListener('mouseup', onUp);
				}
				content.addEventListener('mousemove', onMove, false);
				content.addEventListener('mouseup', onUp, false);
			}
		},
		components: {

		},
		mounted() {

		},
		data() {
			return {

			}
		}
	}
</script>

<style lang="scss" scoped>
	.HorizontalThreeWindowContent {
		display: flex;
		flex-direction: row;
		width: 100%;
		height: 100%;
		overflow: hidden;
		-webkit-user-drag: none;
		position: relative;

		.left: {
			flex-grow: 0 !important;
			flex-shrink: 0 !important;
			min-height: 0;
			min-width: 0;
			overflow: hidden
		}

		.center {
			min-height: 0px;
			min-width: 0px;
			flex-grow: 1;
			flex-shrink: 1;
			overflow: hidden
		}

		.right {
			min-height: 0;
			min-width: 0;
			flex-grow: 0 !important;
			flex-shrink: 0 !important;
			overflow: hidden
		}

		.divide {
			flex-grow: 0;
			flex-shrink: 0;
			display: flex;
			justify-content: center;
			align-items: center;
			height: 100%;
			position: absolute;
			cursor: ew-resize;
			box-sizing: border-box;
			z-index: 999;

			.divideDisplay {
				flex-grow: 0;
				flex-shrink: 0;
				height: 100%;
				background-color: rgba(0, 0, 0, 0.25);
			}
		}
	}
</style>
