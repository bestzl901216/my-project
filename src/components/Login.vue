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
          // this.$http.post('http://jsonplaceholder.typicode.com/todos', this.form)
          this.$router.push('/goods')
        } else {
          console.log('error submit!!')
          return false
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
