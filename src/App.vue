<template>
  <div class="todo">
    <div class="bg_title">
      <h2 v-text="title1"></h2>
    </div>
    <div class="t_input">
      <span><input type="text" class="t_titles" placeholder="请输入数据。。。" v-model="text" @keyup.13="add()" ></span>
      <span><button class="t_add" @click="add()">添加</button></span>
    </div>
    <div class="t-cont">
      <ul>
        <li v-for="(item,index) in todolist" track-by="$index" :key="item.id">
          <input type="checkbox" v-model="item.state" class="checkbox"/>
          {{`${index+1}. `}} {{item.value}}
          <span @click="del(index)">删除</span>
          <span @click="edit(index)">编辑</span>
          <span @click="aldone(index)">完成</span>
        </li>
        <div class="check_che"  v-show="select.length > 0">已选择{{select.length}}项 <span @click="remove()">删除选中</span></div>
        <p v-show="todolist.length == 0">暂无数据！</p>
        <div class="bg_title">
          <h2 v-text="title2"></h2>
        </div>
        <li v-for="(item,index) in alreadylist" track-by="$index" :key="item.id">
          {{`${index+1}. `}} {{item.value}}
          <span @click="deldone(index)">删除</span>
          <span @click="havetodo(index)">未完成</span>
        </li>
        <p v-show="alreadylist.length == 0">暂无数据！</p>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo',
  data () {
    return {
      todolist: [],
      text: '',
      state: false,
      alreadylist: [],
      title1: 'ToDoList',
      title2: 'AlreadyDoneList'
    }
  },
  methods: {
    // 添加未完成
    add () {
      if (this.text === '') {
        return
      } else {
        this.todolist.push({
          value: this.text,
          state: false
        })
      }
      this.text = ''
    },
    // 未完成列表删除
    del (index) {
      this.todolist.splice(index, 1)
    },
    // 未完成列表删除选中
    remove () {
      this.todolist = this.todolist.filter((index) => !index.state)
    },
    // 未完成列表编辑
    edit (index) {
      this.text = this.todolist[index].value
      this.todolist.splice(index, 1)
    },
    // 未完成列表已完成
    aldone (index) {
      this.alreadylist.push({
        value: this.todolist[index].value
      })
      this.todolist.splice(index, 1)
    },
    // 已完成列表删除
    deldone (index) {
      this.alreadylist.splice(index, 1)
    },
    // 已完成列表未完成
    havetodo (index) {
      this.todolist.push({
        value: this.alreadylist[index].value
      })
      this.alreadylist.splice(index, 1)
    }
  },

  // 计算属性
  computed: {
    select () {
      return this.todolist.filter((index) => index.state === true)
    }
  }
}
</script>

<style scoped lang="less">
  @bordercolor:#ccc;
  @checkcolor:deepskyblue;
  .bg_title,.t_input{
    text-align: center;
  }
  .t_titles {
    width: 400px;
    height: 35px;
    font-size: 16px;
    padding-left: 10px;
  }
  .t_add {
    width: 60px;
    height: 40px;
    font-size: 16px;
    vertical-align: middle;
  }
  .t-cont {
    width: 100%;
    margin-top: 20px;
    margin: 0 auto;
    ul li{
      list-style: none;
      height: 30px;
      border: 1px solid @bordercolor;
      line-height: 30px;
      text-align: left;
      padding-left: 10px;
    span{
      float: right;
      cursor: pointer;
      padding-right: 10px;
    }
    input{
      padding-top: 5px;
    }
  }
    ul p{
      text-align: center;
    }
  }
  .checks {
    width: 15px;
    height: 15px;
    border-radius: 50%;
    border: 1px solid @bordercolor;
    display: inline-block;
    margin-top: 5px;
    cursor: pointer;
  }
  .checkbox{
    width: 18px;
    height: 18px;
    border: 1px solid @bordercolor;
    border-radius: 100%;
    position: relative;
    margin-right: 6px;
    vertical-align: middle;
    &:checked{
      border-color: @checkcolor;
      &:before{
        content: '';
        display: block;
        width: 80%;
        height: 80%;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        background: @checkcolor;
        border-radius: 100%;}
    }
  }
  .check_che {
    border: 1px solid @bordercolor;
    padding:10px;
    span{
      float: right;
    }
  }
</style>
