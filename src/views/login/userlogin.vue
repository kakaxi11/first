<template>
<div>
<div>
  <el-form class="login-form" status-icon :rules="loginRules" ref="loginForm" :model="loginForm" label-width="0">
    <el-form-item prop="username">
      <el-input size="small" @keyup.enter.native="handleLogin" v-model="loginForm.username" auto-complete="off" placeholder="请输入用户名">
        <i slot="prefix" class="icon-yonghu"></i>
      </el-input>
    </el-form-item>
    <el-form-item prop="password">
      <el-input size="small" @keyup.enter.native="handleLogin" :type="passwordType" v-model="loginForm.password" auto-complete="off" placeholder="请输入密码">
        <i class="el-icon-view el-input__icon" slot="suffix" @click="showPassword"></i>
        <i slot="prefix" class="icon-mima"></i>
      </el-input>
    </el-form-item>
    <el-checkbox v-model="checked">记住账号</el-checkbox>
    <el-form-item>
      <el-button type="primary" size="small" @click.native.prevent="handleLogin" class="login-submit">登录</el-button>
    </el-form-item>
  </el-form>
</div>






 <el-table
    :data="userList"
    height="250"
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
      prop="sex"
      label="性别">
      <template slot-scope="scope">
{{scope.row.sex ===1?"男":"女"}}
      </template>
    </el-table-column>
    <el-table-column label="操作">
   <el-button type="primary" icon="el-icon-edit" circle @click="edit(scope.row.id)"></el-button>
    </el-table-column>
  </el-table>






</div>
</template>

<script>
import { isvalidUsername } from '@/utils/validate'
export default {
  name: 'userlogin',
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
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      userList:[],
      dialogFormVisible:false,
      checked: false,
      code: {
        src: '',
        value: '',
        len: 4,
        type: 'text'
      },
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
      passwordType: 'password'
    }
  },
  created() {
  },
  mounted() {},
  computed: {
  },
  props: [],
  methods: {
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
<style>
</style>
