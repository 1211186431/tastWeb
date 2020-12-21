<template>
	<div  style="height: 500px;">
		<el-calendar v-model="value">
			<template slot="dateCell" slot-scope="{date, data}">
				<div>
					<div>
					{{ data.day.split('-').slice(2).toString()}}
					</div>
					<div v-show="HaveTask(data)" style="margin-top:15px;">
						<el-tooltip class="item" effect="dark" :content="Tasknum(data)" placement="top-start">
							<el-tag type="danger">任务</el-tag>
						</el-tooltip>
					</div>
				</div>
			</template>
		</el-calendar>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				dialogFormVisible3: false,
				form: {
					formId: "",
					name: "",
					details: "",
					startdate: "",
					enddate: "",
					position: "",
					degree: "",
					completion: 0
				},
				value: new Date(),
				todoData: []
			}
		},
		methods: {
			HaveTask: function(data) {
				if (this.todoData.indexOf(data.day) >= 0) {
					return true;
				} else {
					return false;
				}
			},
			Tasknum: function(data) {
				let num=0;
				for(let i=0;i<this.todoData.length;i++){
					if (this.todoData[i]==(data.day)) {
						num++;
					} 
				}
				
				return "共有"+num+"个任务";
			}
		},
		mounted() {
			var tasklist = JSON.parse(localStorage.getItem('tasklist'));
			for (var i = 0; i < tasklist.length; i++) {
				this.form = tasklist[i];
				var d = this.form.startdate;
				var com=this.form.completion;
				if(com<100){
					this.todoData.push(d);
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
