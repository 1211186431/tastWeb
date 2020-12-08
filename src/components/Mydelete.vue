<template>

		<div id='table'>
			<el-table
		   :data="todoData"
		   style="width: 100%">
			   <el-table-column
				 prop="name"
				 label="任务名称"
				 width="180">
			   </el-table-column>
			   <el-table-column
				 prop="startdate"
				 label="开始日期"
				 width="180">
			   </el-table-column>
		   </el-table >
		 </div>
</template>

<script>
	export default {
	props:['MyData'],
	data(){
		return{
			todoData:""
		};
	},
	created:function(){
		this.todoData=this.MyData;
	}
	}
</script>

<style>
</style>
