<template>
	<header class="header">
		<h1>todos</h1>
		<input id="toggle-all" class="toggle-all" type="checkbox" v-model="isAll">
		<label for="toggle-all"></label>
		<!-- label 可以关联一个表单标签 -->
		<input class="new-todo" placeholder="输入任务名称-回车确认" autofocus @keyup.enter="addFn" v-model="task" />
	</header>
</template>

<script>
export default {
	data() {
		return {
			task: "",
		}
	},
	methods: {
		addFn() {
			if (this.task.trim().length === 0) {
				return alert("任务名不能为空")
			}
			this.$emit("create", this.task)
			this.task = ""
		}
	},
	computed: {
		isAll: {
			set(val) {
				console.log(val);
				this.$parent.list.forEach((ele) => (ele.isDone = val))
			},
			get() {
				return this.$parent.list.every((item) => item.isDone)
			}
		}
	}
}
</script>