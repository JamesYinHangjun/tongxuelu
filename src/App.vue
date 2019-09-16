<template>
  <div id="app">
    <h1>同学录</h1>
    <div class="head">  
      <el-row :gutter="20">
        <el-col :span="6">
          <el-input v-model="user.name" placeholder="请输入姓名"></el-input>
        </el-col>
        <el-col :span="6">
          <el-input v-model="user.gender" placeholder="请输入性别"></el-input>
        </el-col>
        <el-col :span="6">
          <!-- 日期插件 -->
          <el-date-picker
            v-model="user.birthday"
            type="date"
            placeholder="请输入生日"
            format="yyyy 年 MM 月 dd 日"
            value-format="yyyy-MM-dd">
          </el-date-picker>
          <!-- <el-input v-model="user.birthday" placeholder="请输入生日"></el-input> -->
        </el-col>
        <el-col :span="6">
          <el-input v-model="user.phoneNum" placeholder="请输入电话号码"></el-input>
        </el-col>
        
        <el-button type="primary" class="submit" @click="add">提交</el-button>
      </el-row>
    </div>
    <div class="body">
      <el-table
        :data="tableData"
        style="width: 100%">
        <el-table-column
          prop="name"
          label="姓名"
          width="180">
        </el-table-column>
        <el-table-column
          prop="gender"
          label="性别"
          width="180">
        </el-table-column>
        <el-table-column
          prop="birthday"
          label="生日">
        </el-table-column>
        <el-table-column
          prop="phoneNum"
          label="电话号码">
        </el-table-column>

        <!-- 删除按钮 -->
        <el-table-column
          prop="phoneNum"
          label="操作">
          <template slot-scope="scope">
            <el-button type="primary" @click="editUser(scope.row, scope.$index)">修改</el-button>
            <el-button type="danger" round @click="delUser(scope.$index)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // input: "",
      user: {
        name: '',
        gender: '',
        birthday: '',
        phoneNum: ''
      },
      tableData: [{
        name: '王小虎',
        gender: '男',
        birthday: '2016-05-04',
        phoneNum: 15683330121
      },
      {
        name: '王小虎',
        gender: '男',
        birthday: '2016-05-04',
        phoneNum: 15683330121
      },
      {
        name: '王小虎',
        gender: '男',
        birthday: '2016-05-04',
        phoneNum: 15683330121
      }]
    }
  },
  methods: {
    add() {
      if(!this.user.name) {
        this.$message({
          message: '请输入姓名',
          type: 'warning'
        });
        return;
      }
      if(!this.user.gender) {
        this.$message({
          message: '请输入性别',
          type: 'warning'
        });
        return;
      }
      if(!this.user.birthday) {
        this.$message({
          message: '请输入生日',
          type: 'warning'
        });
        return;
      }
      // 对电话号码进行正则校验
      if(!/^1[3456789]\d{9}$/.test(this.user.phoneNum)) {
         this.$message({
          message: '请输入正确的电话号码',
          type: 'warning'
        });
        return;
      } 


      // 添加数据
      this.tableData.push(this.user)
      // 添加完之后清空user
      this.user = {
        name: '',
        gender: '',
        birthday: '',
        phoneNum: ''
      };
      this.$message({
        message: '添加成功!',
        type: 'success'
      })
    },
    // 删除方法
    delUser(index) {
       this.$confirm('确认删除？')
          .then(_ => {
            // done();
            this.tableData.splice(index,1)
          })
          .catch(_ => {});
      }      
    },
    // editUser(user,index) {
      
    // }
  }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 1024px;
  margin: 0 auto;
}
.body {
  margin-top: 20px;
}

/* 按钮 */
.submit {
  width: 100%;
  margin-top: 10px !important;
}

</style>
