<template>
  <div class="todo-footer">
  <label>
    <el-checkbox v-model="checked" @input="chackAll">全选</el-checkbox>
  </label>
  <span>
    <span>已完成 {{count}}</span> / 全部 {{total}}
  </span>

  <el-popconfirm
    confirm-button-text='好的'
    cancel-button-text='不用了'
    icon="el-icon-info"
    icon-color="red"
    title="确定删除已完成任务吗？"
    @confirm="mul_del">
    <el-button
    type="danger" 
    size="mini" 
    slot="reference"
    icon="el-icon-delete">删除已完成任务
    </el-button>
  </el-popconfirm>
  </div>
</template>

<script>
export default {
  name:'UserFooter',
  data() {
    return {
      checked:false,
      count:0,
      total:0,
    }
  },
  methods: {
    chackAll(){
      this.$bus.$emit('chackAll', this.checked)
    },
    mul_del(){
      this.$bus.$emit('mul_del')
    },
    isChack(num, total){
      this.checked = (num == total)
      this.count = num
      this.total = total
    },
  },
  mounted() {
    this.$bus.$on('isChack',this.isChack)
  },
}
</script>

<style>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>