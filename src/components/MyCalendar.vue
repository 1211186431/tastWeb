<template>
	<div>
	<el-calendar v-model="value">
		<template slot="dateCell" slot-scope="{date, data}">
				<div>
					{{ data.day.split('-').slice(2).toString()}}
				<div v-show="HaveTask(data)" style="color: red;" :class="ClickDay(data)">
					<p>有任务</p>
				</div>
				</div>
		</template>
	</el-calendar>
	<el-dialog title="添加任务" :visible.sync="dialogFormVisible3">
		<el-form :model="form">
			<el-form-item label="任务名称">
				<el-input style="width:200px" v-model="form.name"></el-input>
			</el-form-item>
			<el-form-item label="开始日期">
				<el-date-picker v-model="form.startdate" type="datetimerange" range-separator="至" start-placeholder="开始时间"
				 end-placeholder="结束时间" value-format="yyyy-MM-dd HH:mm" format="yyyy-MM-dd HH:mm">
				</el-date-picker>
			</el-form-item>
		</el-form>
		<div slot="footer" class="dialog-footer">
			<el-button @click="dialogFormVisible3 = false">取 消</el-button>
			<el-button type="primary" >确 定</el-button>
		</div>
	</el-dialog>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				dialogFormVisible3: false,
				form: {
					name: "",
					startdate: ""
				},
				value: new Date(),
				todoData:["2020-12-07","2020-12-17","2020-12-27","2021-01-07"]
			}
		},
		methods:{
			HaveTask:function(data){
				if(this.todoData.indexOf(data.day)>=0){
					return true;
				}
				else{
					return false;
				}
			},
			ClickDay:function(data){
				if(data.isSelected){
					this.todoData.push(data);
				}
			}
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	h3 {
		margin: 40px 0 0;
	}

	ul {
		list-style-type: none;
		padding: 0;
	}

	li {
		display: inline-block;
		margin: 0 10px;
	}

	a {
		color: #42b983;
	}
</style>
