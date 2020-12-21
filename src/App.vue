<template>
	<div id="app">
		<div>
			<el-row :gutter="20" class="Header">
				<el-col :span="6" style="height: 75px;">
					<div style="text-align: left;">
						<i class="el-icon-eleme"></i>
						<span>任务管理系统</span>
					</div>
				</el-col>
				<el-col :span="4" style="height: 75px;">
				</el-col>
			</el-row>
		</div>
		<el-container>
			<el-aside style="width: 200px;">
				<Navmenu @MenuItem="changeView"  @Time="setTime" @nsd="setNsd"></Navmenu>
			</el-aside>
			<el-main>
				<MyForm v-if="MyIncrease" :Increase="MyIncrease" @OverIncrease="OverIncrease"></MyForm>
				<component v-if="hackReset" :is="currentView" :Time="Time" :nsd="nsd"></component>
			</el-main>
		</el-container>
		<el-button type="primary" icon="el-icon-edit" circle class="right_bottom" style="font-size: 25px;" @click="Increaseb()" v-show="nsd=='n'"></el-button>
	</div>
</template>

<script>
	import Navmenu from './components/Navmenu.vue'
	import MyCalendar from './components/MyCalendar.vue'
	import MyForm from './components/MyForm.vue'
	import AllList from './components/AllList.vue'
	export default {
		components: {
			Navmenu,
			MyForm,
			AllList,
			MyCalendar
		},
		data() {
			return {
				MyIncrease: false,
				currentView: 'AllList',
				Time: "all",
				hackReset: true,
				nsd:"n"
				
			};
		},
		methods: {
			changeView: function(MenuItem) {
				this.currentView = MenuItem;
			},
			setTime(time) {
				this.myRefresh();
				this.Time = time;
			},
			setNsd(nsd){
                this.myRefresh();
				this.nsd=nsd;
			},
			OverIncrease: function(Increase) {
				this.MyIncrease = Increase;
				this.myRefresh();
			},
			clear_data() {
				localStorage.clear();
				this.myRefresh();
			},
			myRefresh() {
				this.hackReset = false; //强制刷新
				this.$nextTick(() => {
					this.hackReset = true;
				});
			},
			handleCommand(command) {
				switch (command) {
					case "s":
						this.myRefresh();
						break;
					case "p":
						this.$message("这是常见问题解决");
						break;
					case "h":
						this.$message("这是帮助");
						break;
					case "e":
						this.clear_data();
						break;
				}
			},
			Increaseb() {
				this.MyIncrease = true;
			}
		},
		watch: {

		}
	}
</script>

<style>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
	}

	.Header {
		width: 100%;
		height: 75px;
		background-color: #409EFF;
		text-align: center;
		line-height: 4.6875rem;
		font-size: 30px;
	}
	.right_bottom{
		overflow: hidden;
		position: fixed;
		padding:5px;
		text-align:center;
		right: 50px;
		bottom: 50px;
	}
	
</style>
