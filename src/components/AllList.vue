<template>
	<div>
		<el-row>
			<el-col :span="12">
				<div class="grid-content bg-purple">
					<el-input placeholder="请输入内容" class="input-with-select" v-model="serachclick">
						<el-button slot="append" icon="el-icon-search"></el-button>
					</el-input>
				</div>
			</el-col>
			<el-col :span="12">
				<div class="grid-content bg-purple-light">
					<el-select v-model="Posi" filterable placeholder="请选择" clearable>
						<el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
						</el-option>
					</el-select>
				</div>
			</el-col>
		</el-row>
		<MyList :tasklist="plist1">
			<div slot="n" v-if="nsd!='n'"></div>
			<div slot="s" v-if="nsd!='s'"></div>
			<div slot="d" v-if="nsd!='d'"></div>
			
		</MyList>
	</div>
</template>

<script>
	import MyList from './MyList.vue'
	export default {
		props: ['Time', 'nsd'],
		components: {
			MyList
		},
		data() {
			return {
				tasklist: JSON.parse(localStorage.getItem('tasklist')) || [],
				serachclick: "",
				Posi: "",
				form: {
					formId: "",
					name: "",
					details: "",
					startdate: "",
					enddate: "",
					position: "",
					degree: "",
					completion: 0,
					mydate1: "",
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
				plist1: []
			}
		},
		methods: {
			timebool(date, time) {
				var d = date.split('-').join('/'); //改变格式
				var date1 = new Date(d); //起始日期
				var nowdate = new Date(); //当前日期
				var d1 = this.format(nowdate); //转换格式 
				var subday = (date1.getTime() - nowdate.getTime()) / (24 * 3600 * 1000);
				if (time == "all")
					return true;
				if (d1 == date && time == "today") {
					return true;
				}
				if (subday < 7 && time == "week" && subday >= -1) { //相差7天
					return true;
				}
				if (subday < 30 && time == "mouth" && subday >= -1) {
					return true;
				}
				return false;
			},
			format(Date) {
				var Y = Date.getFullYear();
				var M = Date.getMonth() + 1;
				M = M < 10 ? '0' + M : M; // 不够两位补充0
				var D = Date.getDate();
				D = D < 10 ? '0' + D : D;
				return Y + '-' + M + '-' + D;
			},
			serachway() {
				this.plist1 = [];
				if (this.nsd == "d") {
					if (JSON.parse(localStorage.getItem('deletelist')) != null)
						this.plist1 = JSON.parse(localStorage.getItem('deletelist'));
				} else {
					for (var i = 0; i < this.tasklist.length; i++) {
						this.form = this.tasklist[i];
						var d = this.form.name;
						var d2 = this.form.details;
						var p = this.form.position;
						var t = this.form.startdate;
						var c = this.form.completion;			
						if (d.indexOf(this.serachclick) >= 0 || d2.indexOf(this.serachclick) >= 0 || this.serachclick == "") {
							if (this.timebool(t, this.Time)) {
								if (this.Posi == "" || p == this.Posi){
									if (this.nsd == "n") {
										if (c < 100)
											this.plist1.push(this.form);
									} else if (this.nsd == "s") {
										if (c == 100)
											this.plist1.push(this.form);
									}
								}
							}
						}

						this.form = {
							formId: "",
							name: "",
							details: "",
							startdate: "",
							enddate: "",
							position: "",
							degree: "",
							completion: 0
						};
					}
				}
			}
		},
		watch: {
			serachclick(n, o) {
				this.serachway();
			},
			Time() {
				this.serachway();
			},
			Posi() {
				this.serachway();
			},
			nsd() {
				this.serachway();
			}
		},
		mounted() {
			this.serachway();
		}
	}
</script>

<style>
</style>
