<template>
  <div class="login">
    <h1>Welcome My Dear!!!</h1>
    <el-form ref="form" :model="form" :rules="rules" label-width="80px">
      <el-form-item label="账号：" prop="name">
        <el-input v-model="form.name"></el-input>
      </el-form-item>
      <el-form-item label="密码：" prop="pwd">
        <el-input type="password" v-model="form.pwd"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">登录</el-button>
        <el-button>注册</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import api from '@/api'
export default {
  name: 'Login',
  data () {
    return {
      form: {
        name: '',
        pwd: ''
      },
      rules: {
        name: [
          {required: true, message: '请输入账号', trigger: 'blur'},
          { min: 5, message: '长度在5个字符以上', trigger: 'blur' }
        ],
        pwd: [
          {required: true, message: '请输入密码', trigger: 'blur'},
          { min: 5, message: '长度在5个字符以上', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    onSubmit () {
      this.$refs['form'].validate((valid) => {
        if (valid) {
          this.$http.put(api.account.login, this.form).then(res => {
            console.info('res', res)
            if (res.status === 200) {
              this.$router.push('/goods')
            } else {
              this.$message.error('登陆失败')
            }
          })
        } else {
          this.$message.warning('请检查账号密码格式')
        }
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1{
    font-weight: bold;
  }
  .el-form-item {
    margin-bottom: 22px;
    margin-left: auto;
    margin-right: auto;
    width: 300px;
  }
</style>
