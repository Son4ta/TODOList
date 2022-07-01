<template>
  <div>
    <ul class="todo-main">
        <UserItem v-for="todoObj in todos" :key="todoObj.id" :todo="todoObj"></UserItem>
        <el-empty v-if="!this.todos.length" description="在上方添加第一个事项吧！"></el-empty>
    </ul>
  </div>
</template>

<script>
import UserItem from './UserItem.vue'

export default {
  name:'UserList',
  data() {
    return {
      todos:JSON.parse(localStorage.getItem('todos')) || [],
    }
  },
  
  components: {
  UserItem,
  },
  methods: {
    receive(data){
      if(!data.title) {
          this.$notify.error({
            title: 'ERROR',
            message: 'Empty inpute😭',
            duration:'2000',
          })
          return
      }
      this.todos.unshift(data)
      this.$notify({
          title: 'SUCCESS!',
          message: 'Add TODO Item!😀',
          type: 'success'
      })
    },
    del(targetID){
      this.todos = this.todos.filter((item)=>{
        return item.id !== targetID
      })
      this.$message({
          message: '删除成功',
          type: 'success',
          duration:'500',
      });
    },
    mul_del(){
      this.todos = this.todos.filter((item)=>{
        return item.done !== true
      })
      this.$message({
          message: '已删除完成任务！',
          type: 'success',
          duration:'1000',
      });
    },
    chackAll(status){
      this.todos.forEach(element => {
        element.done = status
      });
    },
    itemEdit(targetID, newTitle){
      this.todos.forEach(element => {
        if(element.id == targetID){  
          if(element.title === newTitle){
            return
          }
          else{
            element.title = newTitle
            this.$message({
              message: '修改已应用',
              type: 'success',
              duration:'2000',
            })
          }
        }
      })
    },
  },
  mounted(){
    this.$bus.$on('receive',this.receive)
    this.$bus.$on('del',this.del)
    this.$bus.$on('mul_del',this.mul_del)
    this.$bus.$on('chackAll',this.chackAll)
    this.$bus.$on('itemEdit',this.itemEdit)
  },
  destroyed() {
    this.$bus.$off('receive',this.receive)
    this.$bus.$off('del',this.del)
    this.$bus.$off('mul_del',this.mul_del)
    this.$bus.$off('chackAll',this.chackAll)
    this.$bus.$off('itemEdit',this.itemEdit)
  },
  watch:{
    todos:{
      deep:true,
      handler(newValue){
      localStorage.setItem('todos',JSON.stringify(newValue))
      console.log(1)
      const temp = (newValue.filter((item)=>{
        return item.done == true
      })).length
      this.$bus.$emit('isChack',temp,newValue.length)
    }
    },
  },
}
</script>

<style>
/*main*/
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-main:last-child {
  border-bottom: none;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
} 
</style>