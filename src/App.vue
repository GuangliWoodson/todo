<template>
  <section class="todoapp">
    <!-- 除了驼峰, 还可以使用-转换链接 -->
    <TodoHeader @create="createFn"></TodoHeader>
    <TodoMain :list="showList" @del="delFn"></TodoMain>
    <TodoFooter :count="count" @changeType="changeFn" @cls="cleanFn"></TodoFooter>
  </section>
</template>

<script>
// 1.0 样式引入
import TodoHeader from "./components/TodoHeader";
import TodoMain from "./components/TodoMain";
import TodoFooter from "./components/TodoFooter";


export default {
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter,
  },
  data() {
    return {
      list: (localStorage.getItem("todolist") && JSON.parse(localStorage.getItem("todolist"))) || [],
      getSel: 'all'
    };
  },
  methods: {
    createFn(val) {
      this.list.push({
        id: this.list.length === 0 ? 100 : this.list[this.list.length - 1].id + 1,
        name: val,
        isDone: false
      })
    },
    delFn(id) {
      let index = this.list.findIndex(obj => obj.id === id)
      this.list.splice(index, 1)
    },
    changeFn(val) {
      this.getSel = val
    },
    cleanFn() {
      this.list = this.list.filter((ele) => !ele.isDone)
    }
  },
  computed: {
    count() {
      // !ele.isDone = ele.isDone == false
      return this.list.filter((ele) => !ele.isDone).length
    },
    showList() {
      if (this.getSel === 'yes') {
        return this.list.filter((ele) => ele.isDone)
      } else if (this.getSel === 'no') {
        return this.list.filter((ele) => !ele.isDone)
      } else {
        return this.list
      }
    }
  },
  watch: {
    list: {
      deep: true,
      handler (val) {
        localStorage.setItem("todolist", JSON.stringify(val || []))
      }
    }
  }
};
</script>