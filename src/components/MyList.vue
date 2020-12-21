<template>
	<div id='table'>
		<el-table :data="tasklist1" style="width: 100%" @row-click="onclickRow" height="500">
			<el-table-column prop="degree" width="55">
				<template slot-scope="scope">
					<i class="el-icon-s-flag" :style="myStyle(scope.$index)"></i>
				</template>
			</el-table-column>
			<el-table-column prop="name" label="任务名称" width="180">
			</el-table-column>
			<el-table-column prop="startdate" label="开始日期" width="100">
			</el-table-column>
			<el-table-column prop="enddate" label="结束日期" width="180">
			</el-table-column>
			<el-table-column prop="position" label="情景" width="120">
			</el-table-column>
			<el-table-column prop="completion" label="完成度" width="220">
				<template slot-scope="scope">
					<slot name="n">
						<el-progress type="circle" :percentage="percentage1(scope.$index, tasklist1)" :width="prowidth" :color="Mystatus(scope.$index, tasklist1)"></el-progress>
							<el-button size="mini" @click.stop="addcompl(scope.$index, tasklist1,10)" icon="el-icon-plus" circle></el-button>
							<el-button size="mini" @click.stop="addcompl(scope.$index, tasklist1,-10)" icon="el-icon-minus" circle></el-button>
					</slot>
					<slot name="s">
						<el-progress type="circle" :percentage="percentage1(scope.$index, tasklist1)" :width="prowidth" status="success"></el-progress>
					</slot>
					<slot name="d">
						<el-progress type="circle" :percentage="percentage1(scope.$index, tasklist1)" :width="prowidth" status="warning"></el-progress>
					</slot>
				</template>
			</el-table-column>
			<el-table-column label="操作">
				<template slot-scope="scope">
					<slot name="n">
						<el-button @click.stop="susses(scope.$index, tasklist1)" size="small">
							完成
						</el-button>
						<el-button type="danger" size="small" @click.stop="visible1 = true;visibleindex= scope.$index;">删除</el-button>
					</slot>
					<slot name="s">
						<el-button type="danger" size="small" @click.stop="visible1 = true;visibleindex= scope.$index;">删除</el-button>
					</slot>
					<slot name="d">
						<el-button @click.stop="Recovery(scope.$index, tasklist1)" size="small" type="success">
							恢复
						</el-button>
					</slot>
				</template>
			</el-table-column>
		</el-table>
		<el-dialog title="删除确认" :visible.sync="visible1" width="20%">
			<span>确定要删除吗</span>
			<span slot="footer" class="dialog-footer">
				<el-button @click="visible1 = false">取 消</el-button>
				<el-button type="primary" @click="deleteRow(visibleindex)">确 定</el-button>
			</span>
		</el-dialog>
		<MyForm v-if="MyIncrease" :Increase="MyIncrease" :editform="form" @OverIncrease="OverIncrease"></MyForm>
		<el-dialog title="任务信息" :visible.sync="dialogFormVisible">
			<el-form :model="form" ref="form">
				<el-form-item label="任务名称" prop="name">
					<span>{{form.name}}</span>
				</el-form-item>
				<el-form-item label="任务描述">
					<span>{{form.details}}</span>
				</el-form-item>
				<el-form-item label="开始日期">
					<span>{{form.startdate}}</span>
				</el-form-item>
				<el-form-item label="结束日期">
					<span>{{form.enddate}}</span>
				</el-form-item>
				<el-form-item label="情景">
					<span>{{form.position}}</span>
				</el-form-item>
				<el-form-item label="程度">
					<span>{{form.degree}}</span>
				</el-form-item>
				<el-form-item label="进度">
					<el-progress :percentage="form.completion"></el-progress>
				</el-form-item>
				<el-form-item>
					<el-button @click="dialogFormVisible=false;MyIncrease=true;">编辑</el-button>
					<el-button type="primary" @click="dialogFormVisible=false">返回</el-button>
				</el-form-item>
			</el-form>
		</el-dialog>
	</div>
</template>

<script>
	import MyForm from './MyForm.vue'
	export default {
		components: {
			MyForm
		},
		props: ["tasklist"],
		data() {
			return {
				MyIncrease: false,
				visible1: false,
				visibleindex: 0,
				dialogFormVisible: false,
				prowidth: 50,
				tasklist1: [],
				form: {
					formId: "",
					name: "",
					details: "",
					startdate: "",
					enddate: "",
					position: "",
					degree: "",
					completion: 0,
					mydate1: ""
				},
				percentage: 0,
			}
		},
		methods: {
			deleteRow(index) {
				var deletelist = JSON.parse(localStorage.getItem('deletelist'));
				if (deletelist != null) {
					deletelist.push(this.tasklist1[index]);
					localStorage.setItem('deletelist', JSON.stringify(deletelist));
					this.tasklist1.splice(index, 1);
					localStorage.setItem('tasklist', JSON.stringify(this.tasklist1));
				} else {
					var deletelist = [];
					deletelist.push(this.tasklist1[index]);
					localStorage.setItem('deletelist', JSON.stringify(deletelist));
					this.tasklist1.splice(index, 1);
					localStorage.setItem('tasklist', JSON.stringify(this.tasklist1));
				}
				this.visible1 = false;
				this.$forceUpdate();
			},
			percentage1(index, rows) {
				return this.tasklist1[index].completion;
			},
			susses(index, rows) { //完成
				var id = rows. [index].formId;
				let tasklist = JSON.parse(localStorage.getItem('tasklist'));
				for (var i = 0; i < tasklist.length; i++) {
					if (id == tasklist[i].formId) {
						if (tasklist[i].completion <= 100) {
							tasklist[i].completion = 100;
							rows. [index].completion = 100;
						}
						break;
					}
				}
				localStorage.setItem('tasklist', JSON.stringify(tasklist));
			},
			addcompl(index, rows,p) { //进度变化
				var id = rows. [index].formId;
				let tasklist = JSON.parse(localStorage.getItem('tasklist'));
				for (var i = 0; i < tasklist.length; i++) {
					if (id == tasklist[i].formId) {
						if (tasklist[i].completion < 100) {
							tasklist[i].completion += p;
							rows. [index].completion += p;
						}
						break;
					}
				}
				localStorage.setItem('tasklist', JSON.stringify(tasklist));
			},
			Mystatus(index, rows) { //设置进度条颜色
				var s = this.tasklist1[index].completion;
				if (s < 30) {
					return '#909399';
				} else if (s < 70) {
					return '#e6a23c';
				} else {
					return '#67c23a';
				}
			},
			onclickRow(row) { //有默认参数
				this.form = row;
				this.dialogFormVisible = true;
				//console.log(row);
			},
			myStyle(d) {
				var c = this.tasklist1[d].degree;
				if (c == "高") {
					return "color:#ff0000;font-size: 20px;";
				} else if (c == "中") {
					return "color:#fff35f;font-size: 20px;";
				} else {
					return "color:#c0c0c0;font-size: 20px;";
				}
			},
			OverIncrease: function(Increase) {
				this.MyIncrease = Increase;
			},
			Recovery(index, rows) { //恢复
				var id = rows. [index].formId;
				let deletelist = JSON.parse(localStorage.getItem('deletelist'));
				let tasklist = JSON.parse(localStorage.getItem('tasklist'));
				for (var i = 0; i < deletelist.length; i++) {
					if (id == deletelist[i].formId) {
						tasklist.push(deletelist[i]);
						deletelist.splice(i, 1);
						this.tasklist1.splice(index, 1);
						break;
					}
				}
				localStorage.setItem('tasklist', JSON.stringify(tasklist));
				localStorage.setItem('deletelist', JSON.stringify(deletelist));
				this.$forceUpdate();
			},
		},
		computed: {


		},
		watch: {
			tasklist(newval, oldval) {
				this.tasklist1 = this.tasklist;
			}
		},
		mounted() {
			this.tasklist1 = Object.assign({}, this.tasklist);
		}
	}
</script>

<style>
</style>
