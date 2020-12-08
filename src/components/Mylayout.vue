<template>
	<div>


		<el-button @click="dialogFormVisible = true">添加任务</el-button>
		<el-button @click="sendData">传送数据</el-button>

		<el-dialog title="添加任务" :visible.sync="dialogFormVisible">
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
				<el-button @click="dialogFormVisible = false">取 消</el-button>
				<el-button type="primary" @click="saveTask">确 定</el-button>
			</div>
		</el-dialog>

		<el-dialog title="编辑任务" :visible.sync="dialogFormVisible2">
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
				<el-button @click="dialogFormVisible2 = false">取 消</el-button>
				<el-button type="primary" @click="changeTask">确 定</el-button>
			</div>
		</el-dialog>

		<div id='table'>
			<el-table :data="todoData" style="width: 100%">
				<el-table-column prop="name" label="任务名称" width="180">
				</el-table-column>
				<el-table-column prop="startdate[0]" label="开始日期" width="180">
				</el-table-column>
				<el-table-column prop="startdate[1]" label="结束日期" width="180">
				</el-table-column>
				<el-table-column label="操作" width="120">
					<template slot-scope="scope">
						<el-button @click.native.prevent="lookRow(scope.$index, todoData)" type="text" size="small">
							查看
						</el-button>
						<el-button @click.native.prevent="deleteRow(scope.$index, todoData)" type="text" size="small">
							移除
						</el-button>
					</template>
				</el-table-column>
			</el-table>
		</div>
	</div>
</template>

<script>
	export default {
		props:['MyData'],
		data() {
			return {
				dialogFormVisible: false,
				dialogFormVisible2: false,
				myindex: 0,
				form: {
					name: "",
					startdate: ""
				},
				todoData: [],
				formLabelWidth: '120px',
				deleteData: []
			};
		},
		methods: {
			saveTask() {
				this.todoData.push(this.form);
				this.form = {
					name: "",
					startdate: ""
				};
				this.dialogFormVisible = false;

			},
			changeTask() {
				this.todoData[this.myindex] = this.form;
				this.form = {
					name: "",
					startdate: ""
				};
				this.myindex = 0;
				this.dialogFormVisible2 = false;
			},
			deleteRow(index, rows) {
				//this.deleteData.push(this.todoData.pop());
				console.log(this.todoData[0].name);
				rows.splice(index, 1);
			},
			lookRow(index, rows) {
				this.form.name = this.todoData[index].name;
				this.form.startdate = this.todoData[index].startdate;
				this.myindex = index;
				this.dialogFormVisible2 = true;
			},
			sendData(){
				this.$emit('SendData1',this.todoData);
				//console.log(this.MyData);
			}

		}

	};
</script>

<style>
	.el-header {
		background-color: #B3C0D1;
		color: #333;
		line-height: 60px;
	}

	.el-aside {
		color: #333;
	}
</style>
