<template>
	<div>
		<el-form :model="form">
			<el-form-item label="任务名称">
				<el-input style="width:200px" v-model="form.name"></el-input>
			</el-form-item>
			<el-form-item label="任务描述">
				<el-input style="width:200px" v-model="form.details"></el-input>
			</el-form-item>
			<el-form-item label="开始日期">
				<el-date-picker v-model="form.startdate" type="daterange" align="right" unlink-panels range-separator="至"
				 start-placeholder="开始日期" end-placeholder="结束日期" :picker-options="pickerOptions">
				</el-date-picker>
			</el-form-item>
			<el-form-item label="情景">
				<el-select v-model="form.position" placeholder="请选择情景" filterable allow-create default-first-option clearable>
					<el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
					</el-option>
				</el-select>
			</el-form-item>
			<el-form-item label="程度">
				<el-select v-model="form.degree" placeholder="请选择程度"  clearable :style="DegreeStyle(value)">
					<el-option v-for="item in options2" :key="item.value" :label="item.label" :value="item.value" >
					</el-option>
				</el-select>
			</el-form-item>
		</el-form>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				pickerOptions: {
					shortcuts: [{
						text: '一周',
						onClick(picker) {
							const end = new Date();
							const start = new Date();
							end.setTime(start.getTime() + 3600 * 1000 * 24 * 7);
							picker.$emit('pick', [start, end]);
						}
					}, {
						text: '一个月',
						onClick(picker) {
							const end = new Date();
							const start = new Date();
							end.setTime(start.getTime() + 3600 * 1000 * 24 * 30);
							picker.$emit('pick', [start, end]);
						}
					}, {
						text: '三个月',
						onClick(picker) {
							const end = new Date();
							const start = new Date();
							end.setTime(start.getTime() + 3600 * 1000 * 24 * 90);
							picker.$emit('pick', [start, end]);
						}
					}]
				},
				options: [{
					value: 'home',
					label: '家里'
				}, {
					value: 'office',
					label: '办公室'
				}, {
					value: 'school',
					label: '学校'
				}, {
					value: 'dorm',
					label: '宿舍'
				}, {
					value: 'out',
					label: '外出'
				}],
				options2: [{
					value: 'low',
					label: '低'
				}, {
					value: 'medium',
					label: '中'
				}, {
					value: 'height',
					label: '高'
				}],
				form: {
					name: "",
					details: "",
					startdate: "",
					position: "",
					degree:""
				}
			}
		},
		methods:{
			DegreeStyle(value){
				switch(value){
					case "low":
					return {'color':'blue'};
					break;
					case "medium":
					return {'color':'yellow'};
					break;
					case "height":
					  return {'color':'red'};
					  break;
				}
				
			}
		}
	}
</script>

<style>
</style>
