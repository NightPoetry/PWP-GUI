<template>
	<div class="content">
		<div class="item">
			<HomeItem name="编译项目" :func="compile">
				<img src="../assets/logo.png" />
			</HomeItem>
			<HomeItem name="启动项目" :func="serve">
				<img src="../assets/logo.png" />
			</HomeItem>
		</div>
	</div>
</template>

<script>
	// @ is an alias to /src
	import HomeItem from "@/components/HomeItem.vue"
	let {ipcRenderer} = window.require("electron");
	export default {
		name: 'HomeView',
		components: {
			HomeItem
		},
		data(){
			return {

			}
		},
		methods:{
			compile(){
				this.$router.push('/compileDir')
			},
			serve(){
				this.$router.push('/serve')
			}
		}
	}
</script>
<style scoped lang="scss">
	.content{
		background-color: antiquewhite;
		height: 100vh;
		width: 100vw;
	}
	.item{
		display: flex;
		flex-wrap: wrap;
		height: 100%;
		width: 100%;
		box-sizing: border-box;
		flex-direction: column;
		justify-content: space-around;
		align-items: center;
	}
	img{
		width: 100%;
		height:100%;
	}
</style>
