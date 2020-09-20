<template>
  <div id="table" class="container">
    <h2 class="text-success" style="text-align: center;">To do list</h2>
    <div class="panel-body">
      <div class="input-group">
        <span class="input-group-btn">
          <a class="btn btn-success">任务查询</a>
        </span>
        <input class="form-control" type="text" placeholder="请输入任务情况" v-model="input"/>
      </div>
      <br />
    </div>
    <table class="table table-bordered tabe-hover">
      <tr class>
        <th class="text-center">编号</th>
        <th class="text-center">姓名</th>
        <th class="text-center">任务</th>
        <th class="text-center">完成情况</th>
        <th class="text-center">操作</th>
      </tr>
      <tr v-show="lists.length==0">
        <td colspan="5" class="text-center text-muted">
          <p>暂无数据...</p>
        </td>
      </tr>
      <tr class="text-center" v-for="(list,i) in search" :key="i">
        <td>{{ list.id }}</td>
        <td>{{ list.name }}</td>
        <td>{{ list.task }}</td>
        <td>{{ list.condition }}</td>
        <td>
          <button class="edit btn btn-info btn-sm" @click="openedit(list)">修改任务</button>&nbsp;
          <button class="del btn btn-primary btn-sm" @click="del(i)">删除任务</button>
        </td>
      </tr>
    </table>
    <button class="add btn btn-success" @click="openadd()" style="margin-bottom:10px;">新增任务</button>&nbsp;

    <div id="add" v-show="flag" class="divk">
      <!-- 添加选项 -->
      <h2 class="text-success">添加任务</h2>
      <br />
      <input type="text" class="form-control" aria-label placeholder="请输入您的姓名" v-model="obj.name" />
      <br />
      <input type="text" class="form-control" placeholder="请输入您的任务" v-model="obj.task" />
      <br />
      <input type="text" class="form-control" placeholder="请输入完成情况" v-model="obj.condition" />
      <br />
      <button class="btn btn-success" @click="add()">新增任务</button>&nbsp;&nbsp;&nbsp;
      <button class="btn btn-primary" @click="res()">重置任务</button>&nbsp;&nbsp;&nbsp;
      <button class="btn btn-info" @click="close()">取消选项</button>
    </div>

    <div id="edit" v-show="flag2" class="divk">
      <!-- 修改选项 -->
      <h2 class="text-success">修改任务</h2>
      <br />
      <input
        type="text"
        class="form-control"
        aria-label
        placeholder="请输入您的姓名"
        v-model="editDetail.name"
      />
      <br />
      <input type="text" class="form-control" placeholder="请输入您的任务" v-model="editDetail.task" />
      <br />
      <input type="text" class="form-control" placeholder="请输入完成情况" v-model="editDetail.condition" />
      <br />
      <button class="btn btn-success" @click="update()">修改任务</button>&nbsp;&nbsp;&nbsp;
      <button class="btn btn-info" @click="close2()">取消选项</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      input:'',
      lists: [
        { id: 1, name: "fmq1", task: "抄文言文一篇", condition: "已完成" },
        { id: 2, name: "fmq2", task: "抄文言文二篇", condition: "未完成" },
        { id: 3, name: "fmq3", task: "抄文言文三篇", condition: "已完成" },
        { id: 4, name: "fmq4", task: "抄文言文四篇", condition: "未完成" },
        { id: 5, name: "fmq5", task: "抄文言文五篇", condition: "已完成" },
      ],
      flag: false,
      flag2: false,
      obj: {
        id: "",
        name: "",
        task: "",
        condition: "",
      },
      editDetail: {
        id: "",
        name: "",
        task: "",
        condition: "",
      },
    };
  },
  computed:{
     search:function(){
      console.log("input:",this.input);
      if(!this.input){
        return this.lists;
      };
      return this.lists.filter(v=>{
        return v.condition.includes(this.input)
      });
    },
  },
  methods: {
    //功能方法集合
    openadd() {
      this.flag = true;
      this.flag2 = false;
    },
    add() {
      //添加任务
      if (!this.obj.name || !this.obj.task || !this.obj.condition)
        return alert("输入框不可以为空哦~");
      var _id =
        Math.max(
          ...this.lists.map(function (item) {
            return item.id;
          })
        ) + 1;
      this.lists.push({
        id: _id,
        name: this.obj.name,
        task: this.obj.task,
        condition: this.obj.condition,
      });
      this.obj = {};
    },
    openedit(list) {
      //打开修改栏
      this.flag = false;
      this.flag2 = true;
      this.editDetail = {
        id: list.id,
        name: list.name,
        task: list.task,
        condition: list.condition,
      };
    },
    update() {
      //修改方法
      alert("修改成功！");
      //alert(this.editDetail.id);
      for (var i = 0; i < this.lists.length; i++) {
        if (this.lists[i].id == this.editDetail.id) {
          this.lists[i] = this.editDetail;
          this.flag2 = false;
        }
      }
    },
    del(i) {
      //删除方法
      var con = confirm("确定要删除吗？");
      if (con == true) {
        alert("删除成功！");
        this.lists.splice(i, 1);
      }
    },
    close() {
      //关闭
      this.flag = false;
    },
    close2() {
      //关闭
      this.flag2 = false;
    },
    res() {
      //重置方法
      (this.obj.id = ""),
        (this.obj.name = ""),
        (this.obj.task = ""),
        (this.obj.condition = "");
    }
  },
};
</script>

<style scoped>
th {
  text-align: center;
  background-color: skyblue;
}

tr:nth-of-type(even) {
  background-color: plum;
}

tr:nth-of-type(odd) {
  background-color: pink;
}

.divk {
  border: 2px solid grey;
  border-radius: 2%;
  height: 350px;
}

input {
  width: 50%;
  margin: 0 auto;
}
</style>
