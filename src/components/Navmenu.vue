<template>
	    <el-menu
	      default-active="2"
	      class="el-menu-vertical-demo">
	      <el-submenu index="1">
	        <template slot="title">
	          <i class="el-icon-location"></i>
	          <span>导航一</span>
	        </template>
	        <el-menu-item-group>
	          <template slot="title">分组一</template>
	          <el-menu-item index="1-1" @click="chlickMenu1">今日待办</el-menu-item>
	          <el-menu-item index="1-2" @click="chlickMenu2">日历</el-menu-item>
	        </el-menu-item-group>
	        <el-submenu index="1-4">
	          <template slot="title">选项4</template>
	          <el-menu-item index="1-4-1" @click="chlickMenu3">删除</el-menu-item>
	        </el-submenu>
	      </el-submenu>
	      <el-menu-item index="2">
	        <i class="el-icon-menu"></i>
	        <span slot="title">导航二</span>
	      </el-menu-item>
	      
	    </el-menu>
</template>

<script>
	export default {
		methods:{
			chlickMenu2:function(){
				this.$emit('MenuItem','MyCalendar');
			},
			chlickMenu1:function(){
				this.$emit('MenuItem','Mylayout');
			},
			chlickMenu3:function(){
				this.$emit('MenuItem','Mydelete');
			}
		}
	};
</script>

<style>
</style>
