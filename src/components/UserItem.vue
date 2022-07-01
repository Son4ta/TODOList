<template>
  <div>
    <li>
        <!-- <label> -->
        <el-checkbox 
          v-model="todo.done"
          style="text-decoration:line-through">
          <br>
        </el-checkbox>
        <span 
          v-if="!enableEdit"
          :style="this.itemCss">
          {{todo.title}}
        </span>
        <input
          ref="reference"
          v-if="enableEdit"
          v-model="input" 
          style="display: inline"
          placeholder="请输入内容"
          @blur="subEdit">
        <!-- </label> -->
        <!-- <button class="btn btn-danger">删除</button> -->
        <el-popconfirm
          id="spot"
          confirm-button-text='好的'
          cancel-button-text='不用了'
          icon="el-icon-info"
          icon-color="red"
          title="确定删除这个TODO吗？"
          @confirm="deletItem">
          <el-button
            id="btn"
            type="danger" 
            size="mini" 
            slot="reference"
            icon="el-icon-delete">
          </el-button>
        </el-popconfirm>
        <el-button
          id="btn"
          type="primary" 
          size="mini" 
          icon="el-icon-edit"
          @click="isEdit">
        </el-button>
    </li>
  </div>
</template>

<script>
export default {
    name:'UserItem',
    props:['todo'],
    data() {
      return {
        enableEdit:false,
        input:this.todo.title,
      }
    },
    methods: {
      deletItem(){
        this.$bus.$emit('del',this.todo.id)
      },
      isEdit(){
        this.enableEdit=(!this.enableEdit)
        this.$nextTick(function(){
          this.$refs.reference.focus()
        })
      },
      subEdit(){
        this.$bus.$emit('itemEdit',this.todo.id,this.input)
        this.enableEdit=false
      },
    },
    computed: {
      itemCss:function(){
        if(this.todo.done){
          return {textDecoration:'line-through',
                  color: '#409EFF',}
        }
      }
    },
}
</script>

<style>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

#btn {
  float: right;
  display: none;
  margin-top: 4px;
}

li:before {
  content: initial;
}

/* li:last-child {
  border-bottom: none;
} */

li:hover #btn{
  display: block;
}

el-popconfirm:hover #btn{
  display: block;
}
</style>