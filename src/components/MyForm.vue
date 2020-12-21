<template>
	<div>
		<el-dialog :title="formname" :visible.sync="dialogFormVisible" @close="myclose()">
			<el-form :model="form" :rules="rules" ref="form">
				<el-form-item label="任务名称" prop="name">
					<el-input style="width:200px" v-model="form.name"></el-input>
				</el-form-item>
				<el-form-item label="任务描述">
					<el-input style="width:200px" v-model="form.details"></el-input>
				</el-form-item>
				<el-form-item label="开始日期">
					<el-date-picker v-model="form.mydate1" type="daterange" align="right" unlink-panels range-separator="至"
					 start-placeholder="开始日期" end-placeholder="结束日期" :picker-options="pickerOptions" value-format="yyyy-MM-dd" format="yyyy-MM-dd">
					</el-date-picker>
				</el-form-item>
				<el-form-item label="情景">
					<el-select v-model="form.position" placeholder="请选择情景" filterable allow-create default-first-option clearable>
						<el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
						</el-option>
					</el-select>
				</el-form-item>
				<el-form-item label="程度">
					<el-select v-model="form.degree" placeholder="请选择程度" clearable>
						<el-option v-for="item in options2" :key="item.value" :label="item.label" :value="item.value">
						</el-option>
					</el-select>
				</el-form-item>
				<el-form-item>
					<el-button @click="mycancel()">取 消</el-button>
					<el-button type="primary" @click="submitForm('form')">{{subname}}</el-button>
				</el-form-item>
			</el-form>
		</el-dialog>
	</div>
</template>

<script>
	export default {
		props: ['Increase', 'editform'],
		data() {
			return {
				dialogFormVisible: false,
				formname: "添加任务",
				subname:"立即创建",
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
				options: JSON.parse(localStorage.getItem('MyPosition')) || [{
					value: '家里',
					label: '家里'
				}, {
					value: '办公室',
					label: '办公室'
				}, {
					value: '学校',
					label: '学校'
				}, {
					value: '宿舍',
					label: '宿舍'
				}, {
					value: '外出',
					label: '外出'
				}],
				options2: [{
					value: '低',
					label: '低'
				}, {
					value: '中',
					label: '中'
				}, {
					value: '高',
					label: '高'
				}],
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
				rules: {
					name: [{
						required: true,
						message: '请输入活动名称',
						trigger: 'blur'
					}]
				},
				tasklist: JSON.parse(localStorage.getItem('tasklist')) || []
			}
		},
		mounted() {
			this.dialogFormVisible = this.Increase;
			if (this.editform != null) {
				this.formname = "编辑任务";
				this.subname= "保存";
				this.form=Object.assign({},this.editform);
			}

		},
		watch: {
			Increase(newVal, oldVal) {
				this.dialogFormVisible = newVal;
			},
			editform(newVal, oldVal) {
				//console.log(newVal);
			}

		},
		methods: {
			myclose() {
				this.$emit('OverIncrease', false);
			},
			format(Date) {
				var Y = Date.getFullYear();
				var M = Date.getMonth() + 1;
				M = M < 10 ? '0' + M : M; // 不够两位补充0
				var D = Date.getDate();
				D = D < 10 ? '0' + D : D;
				return Y + '-' + M + '-' + D;
			},
			submitForm(formName) {
				this.$refs[formName].validate((valid) => { //规则检测
					if (valid) {
						if (this.formname == "编辑任务") {
							var id = this.form.formId;
							let tasklist = JSON.parse(localStorage.getItem('tasklist'));
							for (var i = 0; i < tasklist.length; i++) {
								if (id == tasklist[i].formId) {
									const time1 = new Date();
									if (this.form.mydate1 == null || this.form.mydate1 == "") {
										this.form.startdate = this.format(time1);
										this.form.enddate = this.format(time1);
									} else {
										this.form.startdate = this.form.mydate1[0];
										this.form.enddate = this.form.mydate1[1];
									}
									tasklist[i]=this.form;
									break;
								}
							}
							localStorage.setItem('tasklist', JSON.stringify(tasklist));
							this.$emit('OverIncrease', false);
						} else {
							this.form.formId = this.guid();
							const time1 = new Date();
							if (this.form.mydate1 == null || this.form.mydate1 == "") {
								this.form.startdate = this.format(time1);
								this.form.enddate = this.format(time1);
							} else {
								this.form.startdate = this.form.mydate1[0];
								this.form.enddate = this.form.mydate1[1];
							}
							var pan_duan = true; //判断如果不在里面为true 把位置加进去
							for (var i = 0; i < this.options.length; i++) {
								for (var x in this.options[i]) {
									if (x == "value") {
										if (this.form.position == this.options[i][x]) {
											console.log(x + "=" + this.options[i][x]);
											pan_duan = false;
										}
									}
								}
							}
							if (pan_duan && this.form.position != "") {
								this.options.push({
									value: this.form.position,
									label: this.form.position
								});
								localStorage.setItem('MyPosition', JSON.stringify(this.options));
							}
							this.tasklist.push(this.form);
							localStorage.setItem('tasklist', JSON.stringify(this.tasklist));
							this.form = {
								formId: "",
								name: "",
								details: "",
								startdate: "",
								enddate: "",
								position: "",
								degree: "",
								completion: 0,
								mydate1: ""
							};
							this.$emit('OverIncrease', false);
							 this.$message('创建成功');
						}
						//location.reload(0);
					} else {
						console.log('error submit!!');
						return false;
					}
				});
			},
			mycancel(){
				this.form= this.editform;
				this.$emit('OverIncrease',false);
			},
			guid() {
				function S4() {
					return (((1 + Math.random()) * 0x10000) | 0).toString(16).substring(1);
				}
				return (S4() + S4() + "-" + S4() + "-" + S4() + "-" + S4() + "-" + S4() + S4() + S4());
			}
		}
	}
</script>

<style>
</style>
