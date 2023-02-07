<template>
	<div class="content">
		<div class="items">
			<span>
				运行中的项目
			</span>
			<template v-for="item in items">
				<p> {{item}} </p>
			</template>
		</div>
		<div class="setarea">
			<div class="message">
				{{message}}
			</div>
			<div class="drag_in" @dragover="dragover_func" @drop="source_drop_func">
				<p>要运行的项目文件夹</p>
				<p class="notice">拖拽到此处</p>
			</div>
		</div>
	</div>
</template>

<script>
	let {
		ipcRenderer
	} = window.require("electron");
	let Path = window.require("path");
	export default {
		name: "Serve",
		components: {

		},
		data() {
			return {
				items: [],
				message: "",
			}
		},
		mounted() {
			ipcRenderer.on("finished", () => {
				this.message = "部署完成！请查收QWQ！";
				setTimeout(()=>{
					this.message = "";
				},5000);
			});
		},
		methods: {
			dragover_func(ev) {
				ev.preventDefault();
			},
			source_drop_func(ev) {
				let path = ev.dataTransfer.files[0].path;
				for(let p of this.items){
					if(p == Path.basename(path)){
						this.message=`"${Path.basename(p)}"项目已存在`;
						setTimeout(()=>{
							this.message="";
						},5000);
						ev.preventDefault();
						return;
					}
				}
				ipcRenderer.send("serve", path);
				this.items.push(Path.basename(path));
				ev.preventDefault();
			},
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
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		font-size: 30px;
		font-weight: 900;
		color: yellowgreen;
		width: 100%;
	}

	.notice {
		color: green;
		font-family: "宋体";
		font-size: 20px;
	}

	.back {
		position: fixed;
		bottom: 10px;
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100px;
		width: 100%;

		img {
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
		overflow: hidden;

		.items {
			height: 100%;
			width: 250px;
			border-right: 3px dashed green;
			scroll-behavior: smooth;
			overflow: scroll;

			&::-webkit-scrollbar {
				display: none;
			}

			p {
				display: flex;
				flex-direction: row;
				justify-content: flex-start;
				align-items: flex-end;
				width: 100%;
				height: 30px;
				border-bottom: 2px pink solid;
				box-sizing: border-box;
				padding: 5px;
				color: tomato;
				font-size: 18px;
				font-weight: 900;
				font-family: "仿宋";
			}

			span {
				position: sticky;
				top: 0;
				background-color: antiquewhite;
				display: flex;
				flex-direction: row;
				justify-content: center;
				align-items: center;
				width: 100%;
				height: 30px;
				border-bottom: 2px pink solid;
				box-sizing: border-box;
				color: tomato;
				font-size: 18px;
				font-weight: 900;
				font-family: "仿宋";
				user-select: none;
			}
		}

		.setarea {
			height: 100%;
			width: 100%;
			display: flex;
			justify-content: center;
			align-items: center;

			div {
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
				font-size: 30px;
				font-weight: 900;
				color: yellowgreen
			}

			.drag_in {
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
				border: orange 2px dashed;
				border-radius: 30px;
				transition: all 0.5s;

				&:hover {
					background-color: rgba(255, 255, 255, 0.4);
					border-color: blueviolet;
				}
			}
		}
	}
</style>
