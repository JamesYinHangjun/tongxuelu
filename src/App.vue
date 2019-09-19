<template>
  <div id="app">
    <h1>同学录</h1>
    <!-- head 部分   填写信息部分 -->
    <div class="head"> 
      <!-- LayOut布局  -->
      <el-row :gutter="20">
        <el-col :span="6">
          <!-- input输入框 -->
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
        
        <!-- 提交按钮 -->
        <el-button type="primary" class="submit" @click="add">提交</el-button>
      </el-row>
    </div>

    <!-- body部分 -->
    <div class="body">
      <!-- table表格 -->
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

        <!-- 删除和修改按钮 -->
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

    <!-- 修改的对话框 -->
    <el-dialog
      title="编辑"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose">

      <p>
        <el-form ref="form"  label-width="80px">
          <el-form-item label="姓名">
            <el-input v-model="objUser.name" placeholder="请输入姓名"></el-input>
          </el-form-item>
          <el-form-item label="性别">
            <el-input v-model="objUser.gender" placeholder="请输入性别"></el-input>
          </el-form-item>
          <el-form-item label="生日">
            <el-date-picker
              v-model="objUser.birthday"
              type="date"
              placeholder="请输入生日"
              format="yyyy 年 MM 月 dd 日"
              value-format="yyyy-MM-dd">
            </el-date-picker>
          </el-form-item>
          <el-form-item label="电话号码">
            <el-input v-model="objUser.phoneNum" placeholder="请输入正确的电话号码"></el-input>
          </el-form-item>
        </el-form>
      </p>

      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="confirm">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // input: "",
      dialogVisible: false,          // 控制编辑的对话框的
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
        phoneNum: 15683399789
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
      }],
      // 这个是定义的修改的弹出框的对象内容
      objUser: {
        name: '',
        gender: '',
        birthday: '',
        phoneNum: ''
      },
      index: 0
    }
  },
  methods: {
    // 点击按钮添加
    add() {
      if(!this.user.name) {
        // ElementUI中的Message消息提示
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
       // Dialog对话框
      this.$confirm('确认删除？')
        .then(_ => {
          // done();
          this.tableData.splice(index,1)
        })
        .catch(_ => {});
    },
    editUser(user,index) {
      this.index = index
      // 点击编辑按钮，会显示出编辑对话框
      this.dialogVisible = true
      // 这么做是 当 点击修改按钮出现对话框时，原来的内容会显示
      this.objUser = {
        name: user.name,
        gender: user.gender,
        birthday: user.birthday,
        phoneNum: user.phoneNum
      }
    },
    // 修改数据
    confirm() {
      // 修改数组数据
      // Vue.$set(要修改的数据)
      this.$set(this.tableData, this.index, this.objUser)
      this.dialogVisible = false
      this.$message({
        message: '数据修改成功!',
        type: 'success'
      })
    },
    handleClose() {
      this.dialogVisible = false
    }
    }
    
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
