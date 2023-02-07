<template>
	<div class="item" @click="func">
		<div class="img_div">
			<slot></slot>
		</div>
		<span>{{name}}</span>
	</div>
</template>

<script>
	export default {
		name: 'HomeItem',
		components: {

		},
		data() {
			return {

			}
		},
		props: {
			name: String,
			func: Function
		}
	}
</script>

<style scoped lang="scss">
	.item {
		max-height: 300px;
		max-width: 300px;
		display: flex;
		flex-direction: column;
		border-radius: 30px;
		background-color: rgba(255, 150, 255, 0.3);
		span {
			flex-basis: 0;
			font-size: 30px;
			font-family: "楷体";
			font-weight: 900;
		}

		.img_div {
			flex-basis: 0;
		}

		box-sizing: border-box;
		border: 2px green solid;
		transition: all 0.5s;
		&:hover{
			background-color: snow;
			border-color: whitesmoke;
		}
	}
</style>
