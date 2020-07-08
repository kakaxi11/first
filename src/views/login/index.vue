<template>
  <div class="login-container pull-height" @keyup.enter.native="handleLogin">
    <div class="login-info text-white animated fadeInLeft">



<el-dialog title="修改用户信息" :visible.sync="dialogFormVisible">
  <el-form :model="editform">
    <el-form-item label="用户id" :label-width="formLabelWidth">
      <el-input v-model="editform.id" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="用户姓名" :label-width="formLabelWidth">
      <el-input v-model="editform.name" autocomplete="off"></el-input>
    </el-form-item>
     <el-form-item label="用户邮箱" :label-width="formLabelWidth">
      <el-input v-model="editform.email" autocomplete="off"></el-input>
    </el-form-item>
      <el-form-item label="用户电话" :label-width="formLabelWidth">
      <el-input v-model="editform.phone" autocomplete="off"></el-input>
    </el-form-item>


    <el-form-item label="用户性别" :label-width="formLabelWidth">
      <el-select v-model="editform.sex" placeholder="请选择用户性别">
        <el-option label="男" :value="1"></el-option>
        <el-option label="女" :value="0"></el-option>
      </el-select>
    </el-form-item>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisible = false">取 消</el-button>
    <el-button type="primary" @click="editSend">确 定</el-button>
  </div>
</el-dialog>

<el-dialog title="新增用户" :visible.sync="adddialogFormVisible">
  <el-form :model="addform">
    <el-form-item label="用户id" :label-width="formLabelWidth">
      <el-input v-model="addform.id" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="用户姓名" :label-width="formLabelWidth">
      <el-input v-model="addform.name" autocomplete="off"></el-input>
    </el-form-item>
     <el-form-item label="用户邮箱" :label-width="formLabelWidth">
      <el-input v-model="addform.email" autocomplete="off"></el-input>
    </el-form-item>
      <el-form-item label="用户电话" :label-width="formLabelWidth">
      <el-input v-model="addform.phone" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="用户性别" :label-width="formLabelWidth">
      <el-select v-model="addform.sex" placeholder="请选择用户性别">
        <el-option label="男" :value="1"></el-option>
        <el-option label="女" :value="0"></el-option>
      </el-select>
    </el-form-item>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisible = false">取 消</el-button>
    <el-button type="primary" @click="addSend">确 定</el-button>
  </div>
</el-dialog>



      <div class="logo" style="margin-top:-426px">
          <img src="../../assets/images/home/logo.png" width="160px" height="160px" alt="logo" style="vertical-align: middle;" />
      </div>
      <h2 class="login-info-title">{{website.info.title}}</h2>
      <ul class="login-info-list">
        <li class="login-info-item" v-for="item in website.info.list">
          <i class="el-icon-check"></i>&nbsp;{{item}}
        </li>
      </ul>
    </div>
    <div class="login-border  animated fadeInRight">
      <div class="login-main">
        <h4 class="login-title">登录{{website.title}}
        </h4>
        <el-tabs v-model="activeName">
          <el-tab-pane label="用户列表" name="user">
         
            <div>
<div>
  <el-form class="login-form" status-icon :rules="loginRules" ref="loginForm" :model="loginForm" label-width="0">
    <!-- <el-form-item prop="username">
      <el-input size="small" @keyup.enter.native="handleLogin" v-model="loginForm.username" auto-complete="off" placeholder="请输入用户名">
        <i slot="prefix" class="icon-yonghu"></i>
      </el-input>
    </el-form-item>
    <el-form-item prop="password">
      <el-input size="small" @keyup.enter.native="handleLogin" :type="passwordType" v-model="loginForm.password" auto-complete="off" placeholder="请输入密码">
        <i class="el-icon-view el-input__icon" slot="suffix" @click="showPassword"></i>
        <i slot="prefix" class="icon-mima"></i>
      </el-input>
    </el-form-item> -->
    <!-- <el-checkbox v-model="checked">记住账号</el-checkbox> -->
    <el-form-item>
      <el-button type="primary" size="small" @click.native.prevent="handleLogin" class="login-submit">获取用户列表</el-button>
      <el-button @click="add">新增用户</el-button>
    </el-form-item>
  </el-form>
</div>






 <el-table
    :data="userList"
    height="500"
    border
    style="width: 100%">
    <el-table-column
      prop="id"
      label="用户ID"
      width="80">
    </el-table-column>
    <el-table-column
      prop="name"
      label="用户姓名"
      width="150">
    </el-table-column>
    <el-table-column
      prop="email"
      label="邮箱"
      width="180"
      >
    </el-table-column>
     <el-table-column
      prop="phone"
      label="电话"
      width="180"
      >
    </el-table-column>
       <el-table-column
      prop="sex"
      label="性别">
      <template slot-scope="scope">
{{scope.row.sex ==1?"男":"女"}}
      </template>
    </el-table-column>
    <el-table-column label="操作"
    width="118"
    >
      <template slot-scope="scope">
   <el-button type="primary" icon="el-icon-edit" circle @click="edit(scope.row.id)" size="small"></el-button>
   <el-button type="danger" icon="el-icon-delete" circle @click="delet(scope.row.id)" size="small"></el-button>
      </template>
    </el-table-column>
  </el-table>






</div>
          </el-tab-pane>
          <el-tab-pane label="短信验证码" name="code">
            <codeLogin></codeLogin>
          </el-tab-pane>
        </el-tabs>
      </div>
    </div>

  </div>
</template>
<script>
import codeLogin from './codelogin'
import { mapGetters } from 'vuex'
import { isvalidUsername } from '@/utils/validate'
export default {
  name: 'login',
  components: {

    codeLogin
  },
  data() {
      const validateUsername = (rule, value, callback) => {
      if (!isvalidUsername(value)) {
        callback(new Error('请输入正确的用户名'))
      } else {
        callback()
      }
    }
    const validateCode = (rule, value, callback) => {
      if (this.code.value !== value) {
        this.loginForm.code = ''
        this.refreshCode()
        callback(new Error('请输入正确的验证码'))
      } else {
        callback()
      }
    }
    return {
      activeName: 'user',
         loginForm: {
        username: 'admin',
        password: '123456'
      },
      addform:{},
      deleform:{
        id:null
      },
      userList:[],
      dialogFormVisible:false,
      adddialogFormVisible:false,
      checked: false,
      code: {
        src: '',
        value: '',
        len: 4,
        type: 'text'
      },
      editform:{},
      loginRules: {
        username: [
          { required: true, trigger: 'blur', validator: validateUsername }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, message: '密码长度最少为6位', trigger: 'blur' }
        ],
        code: [
          { required: true, message: '请输入验证码', trigger: 'blur' },
          { min: 4, max: 4, message: '验证码长度为4位', trigger: 'blur' },
          { required: true, trigger: 'blur', validator: validateCode }
        ]
      },
      passwordType: 'password',
      formLabelWidth:'80px'
      
    }
  },
  created() {},
  mounted() {},
  computed: {
    ...mapGetters(['website'])
  },
  props: [],
  methods: {
    delet(id){
      // this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
      //   confirmButtonText: '确定',
      //   cancelButtonText: '取消',
      //   type: 'warning'
      // })
      //   .then(() => {
        this.deleform.id = id
this.$http.delete("http://127.0.0.1:8080/delet",JSON.stringify(this.deleform)).then(()=>{
  this.getlogList()
})
  //  this.$message({
  //           type: 'success',
  //           message: '删除成功!'
  //         });
  //       })
  //       .catch(() => {
  //         this.$message({
  //           type: 'info',
  //           message: '已取消删除'
  //         })
  //       })
    },
    add(){
      this.adddialogFormVisible = true;
    },
    // 新增用户
    addSend(){
    this.$http.post('http://127.0.0.1:8080/adds',JSON.stringify(this.addform)).then(res=>{
      this.$message.success("新增成功");
      this.adddialogFormVisible =false;
      this.handleLogin()
    })
    },
 edit(id){
      // 拿到当前对象
      let idnex = this.userList.findIndex(item=>{
        return item.id ===id;
      })
      this.editform = this.userList[idnex]
      this.dialogFormVisible = true;
    },
    showPassword() {
      this.passwordType === ''
        ? (this.passwordType = 'password')
        : (this.passwordType = '')
    },
    //点击完成修改
editSend(){
  this.dialogFormVisible = false

// JSONstringify将值转换为json字符串
  this.$http.post('http://127.0.0.1:8080/edit',JSON.stringify(this.editform)).then(res=>{
    this.$http.success('修改成功');
  })
  //首先从servlet获取到传过去的对象，
  // 然后再servlet里调用修改数据库函数，函数的参数为传过去的对象，再修改一下数据库函数，把值都动态绑定。









},

    handleLogin() {
    //   this.$refs.loginForm.validate(valid => {
    //     if (valid) {
    //       this.$store.dispatch('Login', this.loginForm).then(res => {
    //         this.$router.push({ path: '/dashboard/dashboard' })
    //       })
    //     }
    //   })
      this.$http.get('http://127.0.0.1:8080/foot').then(res=>{
    console.log(res);
    this.loginForm.username = res.data[0].name
    this.loginForm.password = res.data[0].phone
    this.userList = res.data
      })
    }

  }
}
</script>

<style lang="scss">
.login-container {
  display: flex;
  align-items: center;
  justify-content: space-around;
  background: rgba(0, 0, 0, 0.2);
  position: relative;
  height: 900px;
}
.login-container::before {
  z-index: -999;
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-image: url("../../assets/images/top_images/login.png");
  background-size: cover;
}
.login-info {
  padding-left: 60px;
}
.login-info-title {
  line-height: 90px;
}
.login-info-item {
  font-size: 14px;
}
.login-border {
  display: flex;
  justify-content: center;
  flex-direction: column;
  padding: 30px 50px 25px 50px;
  background-color: #fff;
  border-radius: 6px;
  box-shadow: 1px 1px 2px #eee;
}
.login-main {
  border-radius: 3px;
  box-sizing: border-box;
  background-color: #fff;
}
.login-main > h3 {
 
}
.login-main > p {
  color: #76838f;
}
.login-title {
  // margin: 0 0 20px;
  text-align: center;
  color: #409eff;
  letter-spacing: 3px;
}
.login-submit {
  margin-top: 0px;
  border-radius: 28px;
}
.login-form {
  margin: 10px 0;
  .el-form-item__content {
    width: 270px;
  }
  .el-form-item {
    margin-bottom: 12px;
  }
  .el-input {
    input {
      text-indent: 5px;
      border-color: #dcdcdc;
      border-radius: 3px;
    }
    .el-input__prefix {
      i {
        padding: 0 5px;
        font-size: 16px !important;
      }
    }
  }
}
.login-code {
  display: flex;
  align-items: center;
  justify-content: space-around;
  margin: 0 0 0 10px;
}
.login-code-img {
  margin-top: 2px;
  width: 100px;
  height: 32px;
  background-color: #fdfdfd;
  border: 1px solid #f0f0f0;
  color: #333;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 5px;
  line-height: 32px;
  text-indent: 5px;
  text-align: center;
}
</style>