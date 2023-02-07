<template>
	<div class="content">
		<div class="back">
			<img src="../assets/logo.png" @click="this.$router.go(-1)">
		</div>
		<div class="message">
			{{message}}
		</div>
		<div class="sourceDir" @dragover="dragover_func" @drop="source_drop_func">
			<p> 被编译的文件夹</p>
			<p class="notice">拖拽到此处</p>
		</div>
		<div class="targetDir" @dragover="dragover_func" @drop="target_drop_func">
			<p>编译后存放文件夹</p>
			<p class="notice">拖拽到此处</p>
		</div>
	</div>
</template>

<script>
	let {
		ipcRenderer
	} = window.require("electron");
	export default {
		name: 'CompileDir',
		components: {

		},
		data() {
			return {
				message: "",
				source: false,
				target: false,
				source_path: "",
				target_path: ""
			}
		},
		methods: {
			compile() {

			},
			dragover_func(ev) {
				ev.preventDefault();
			},
			source_drop_func(ev) {
				let path = ev.dataTransfer.files[0].path;
				this.source_path = path;
				if (!this.target) {
					this.message = "请放置编译后文件夹"
					this.source = true;
				} else {
					this.message = "正在编译请稍后"
					ipcRenderer.send("compile_dir", {
						target_path: this.target_path,
						source_path: this.source_path
					});
				}
				ev.preventDefault();
			},
			target_drop_func(ev) {
				let path = ev.dataTransfer.files[0].path;
				this.target_path = path;
				if (!this.source) {
					this.message = "请放置被编译文件夹"
					this.target = true;
				} else {
					this.message = "正在编译请稍后"
					ipcRenderer.send("compile_dir", {
						target_path: this.target_path,
						source_path: this.source_path
					});
				}
				ev.preventDefault();
			}
		},
		mounted() {
			ipcRenderer.on("finished", () => {
				this.message = "编译完成！请查收QWQ！";
				this.source = false;
				this.target = false;
				this.source_path = "";
				this.target_path = "";
			});
		}
	}
</script>

<style scoped lang="scss">
	.message {
		position: fixed;
		display: flex;
		justify-content: center;
		align-items: center;
		top: 100px;
		user-select: none;
	}

	.notice {
		color: green;
		font-family: "宋体";
		font-size: 20px;
	}
	.back{
		position: fixed;
		bottom: 10px;
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100px;
		width: 100%;
		img{
			height: 100%;
		}
	}
	.content {
		height: 100vh;
		width: 100vw;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: center;
		background-color: antiquewhite;

		div {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			font-size: 30px;
			font-weight: 900;
			color: yellowgreen
		}

		.sourceDir {
			user-select: none;
			margin: 10px;
			flex-grow: 1;
			flex-shrink: 1;
			flex-basis: 0;
			min-height: 200px;
			min-width: 200px;
			max-height: 400px;
			max-width: 400px;
			background-color: rgba(255, 255, 255, 0.1);
			border: white 2px dashed;
			border-radius: 30px;
			transition: all 0.5s;

			&:hover {
				background-color: rgba(255, 255, 255, 0.4);
				border-color: blueviolet;
			}
		}

		.targetDir {
			user-select: none;
			margin: 10px;
			flex-shrink: 1;
			flex-grow: 1;
			flex-basis: 0;
			height: 200px;
			width: 200px;
			max-height: 400px;
			max-width: 400px;
			background-color: rgba(255, 255, 255, 0.1);
			border: white 2px dashed;
			border-radius: 30px;
			transition: all 0.5s;

			&:hover {
				background-color: rgba(255, 255, 255, 0.4);
				border-color: blueviolet;
			}
		}
	}
</style>
