<template>
	<div class="SingleLabelCardContent">
		<div class="tag" draggable="false" style="font:caption">{{tagName}}</div>
		<div class="inContent">
			<slot></slot>
		</div>
	</div>
</template>

<script>
	export default {
		name: "SingleLabelCard",
		props: {
			tagName: {
				type: String,
				default: " "
			}
		},
		methods: {

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
	.SingleLabelCardContent {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		position: relative;

		.tag {
			flex-shrink: 0;
			flex-grow: 0;
			height: 30px;
			width: 100%;
			display: flex;
			justify-content: center;
			align-items: center;
			background-color: rgba(0, 0, 0, 0.15);
			color: rgba(0, 0, 0, 0.9);
			font-family: 楷体;
			font-weight: 800;
			font-size: 20px;
			user-select: none;
			overflow: hidden;
			flex-wrap: nowrap;
			white-space: nowrap;
			text-overflow: ellipsis;
		}

		.inContent {
			flex-shrink: 1;
			flex-grow: 1;
			display: flex;
			position: relative;
			overflow: hidden;
		}
	}
</style>
