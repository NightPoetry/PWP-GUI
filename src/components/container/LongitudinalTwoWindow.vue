<template>
	<div class="LongitudinalTwoWindowContent" ref="content">
		<div ref="up" :style="{'height':upHeight+'px'}" class="up">
			<slot name="up"></slot>
		</div>
		<div class="divide" draggable="false" @mousedown="divideMouseDown($event)" :style="{'height':ply+'px','top':(upHeight-(ply/2))+'px'}">
			<div class="divideDisplay" :style="{'height':plyDisplay+'px'}"></div>
		</div>
		<div ref="down" class="down">
			<slot name="down"></slot>
		</div>
	</div>
</template>

<script>
	export default {
		name: "LongitudinalTwoWindow",
		props: {
			ply: {
				type: Number,
				default: 4
			},
			plyDisplay: {
				type: Number,
				default: 2
			},
			upHeight: {
				type: Number,
				default: 400
			}
		},
		methods: {
			divideMouseDown(event) {
				let position = event.clientY;
				let divide = event.currentTarget;
				let up = this.$refs['up'];
				let content = this.$refs['content'];
				let upHeight = up.offsetHeight;
				let divideTop = parseInt(divide.style.top);

				function onMove(e) {
					let nowPosition = e.clientY;
					let d = nowPosition - position;
					up.style.height = (upHeight + d) + 'px';
					divide.style.top = (divideTop + d) + 'px';
				}

				function onUp() {
					content.removeEventListener("mousemove", onMove);
					content.removeEventListener('mouseup', onUp);
				}
				content.addEventListener("mousemove", onMove, false);
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
	.LongitudinalTwoWindowContent {
		display: flex;
		flex-direction: column;
		height: 100%;
		width: 100%;
		position: relative;

		.up {
			flex-grow: 0;
			flex-shrink: 0;
			width: 100%;
			overflow: hidden
		}

		.down {
			flex-grow: 1;
			flex-shrink: 1;
			width: 100%;
			overflow: hidden
		}

		.divide {
			flex-grow: 0;
			flex-shrink: 0;
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			position: absolute;
			cursor: ns-resize;
			z-index: 999;

			.divideDisplay {
				flex-grow: 0;
				flex-shrink: 0;
				width: 100%;
				background-color: rgba(0, 0, 0, 0.25);
			}
		}
	}
</style>
