<template>
	<div id="app">
		<el-container>
			<el-header>Header</el-header>
			<el-container>
				<el-aside width="200px">
				<Navmenu @MenuItem="changeView"></Navmenu>
				</el-aside>
				<el-main>
					<component :is="currentView" :MyData="todoData" @SendData1="SendData"></component>
				</el-main>
			</el-container>
		</el-container>
		<!-- <HelloWorld></HelloWorld> -->
	</div>
</template>

<script>
	import Navmenu from './components/Navmenu.vue'
	import Mylayout from './components/Mylayout.vue'
	import MyCalendar from './components/MyCalendar.vue'
	import Mydelete from './components/Mydelete.vue'
	import MyForm from './components/MyForm.vue'
    
	export default {
		components: {
			 Navmenu,
			Mylayout,
			MyCalendar,
			Mydelete,
			MyForm
		},
		data() {
			return {
				currentView:'MyForm',
				todoData:""
			};
		},
		methods:{
			changeView:function(MenuItem){
				this.currentView=MenuItem;
			},
			SendData:function(Mydata){
				this.todoData=Mydata;
			}
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
</style>
