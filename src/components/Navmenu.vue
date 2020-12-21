<template>
	<el-menu default-active="2" class="el-menu-vertical-demo">
		<el-menu-item-group class="group1">
			<el-menu-item index="1-0" @click="chlickMenuT(all)" class="item1"><i class="el-icon-menu" style="font-size: 25px;"></i>全部待办</el-menu-item>
			<el-menu-item index="1-1" @click="chlickMenuT(today)" class="item1"><i class="el-icon-star-on" style="font-size: 25px;"></i>今日待办</el-menu-item>
			<el-menu-item index="1-2" @click="chlickMenuT(week)" class="item1"><i class="el-icon-notebook-1" style="font-size: 25px;"></i>周待办</el-menu-item>
			<el-menu-item index="1-3" @click="chlickMenuT(mouth)" class="item1"><i class="el-icon-notebook-2" style="font-size: 25px;"></i>月待办</el-menu-item>
			<el-menu-item index="1-4" @click="chlickMenu2()" class="item1"><i class="el-icon-date" style="font-size: 25px;"></i>日历</el-menu-item>
			<el-menu-item index="1-5" @click="chlickSuss()" class="item1"><i class="el-icon-s-claim" style="font-size: 25px;"></i>已完成</el-menu-item>
			<el-menu-item index="1-6" @click="chlickDelete()" class="item1"><i class="el-icon-delete" style="font-size: 25px;"></i>已删除</el-menu-item>
		</el-menu-item-group>
	</el-menu>
</template>

<script>
	export default {
		data() {
			return {
				all: "all",
				today: "today",
				week: "week",
				mouth: "mouth"
			}

		},
		methods: {
			chlickMenu2: function() {
				this.$emit('MenuItem', 'MyCalendar');
			},
			chlickMenuT(time) {
				this.$emit('MenuItem', 'AllList');
				this.$emit('Time', time);
				this.$emit('nsd', "n");
			},
			chlickSuss() {
				this.$emit('MenuItem', 'AllList');
				this.$emit('nsd', "s");
			},
			chlickDelete() {
				this.$emit('MenuItem', 'AllList');
				this.$emit('nsd', "d");
			}

		}
	};
</script>

<style>
	.group1 {
		font-size: 15px;
		text-align: left;
	}

	.item1 {
		text-align: left;
		font-size: 18px;
	}

	.item2 {
		font-size: 25px;
	}
</style>
