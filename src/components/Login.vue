<template>
  <div class="login">
    <el-form ref="userFormRef" :model="userForm" :rules="userFormRules" label-width="0px" class="user_form">
      <!-- 用户名 -->
      <el-form-item prop="username">
        <el-input
            v-model="userForm.username" placeholder="username"
        >
        </el-input>
      </el-form-item>
      <!-- 密码 -->
      <el-form-item prop="password">
        <el-input
            v-model="userForm.password" type="password" placeholder="password" show-password
        >
        </el-input>
      </el-form-item>
      <!-- 按钮区域 -->
      <el-row justify="center">
        <el-form-item class="user_btn">
          <el-button type="primary" @click="login()">登录</el-button>
          <el-button type="info" @click="resetUserForm">重置</el-button>
        </el-form-item>
      </el-row>
    </el-form>
  </div>
  <div class="msg">
    <p>{{msg}}</p>
    <p>{{uid}}</p>
    <p>{{token}}</p>
  </div>
</template>

<script>
  const axios=require('axios').default
  export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: "Login",
    data(){
      return {
        // 这是表单的数据绑定对象
        userForm: {
          username: '',
          password: ''
        },
        // 这是表单的验证规则对象
        userFormRules: {
          // 验证用户名是否合法
          username: [
            {required: true, message: '不能为空', trigger: 'blur'},
            {min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur'}
          ],
          // 验证密码是否合法
          password: [
            {required: true, message: '不能为空', trigger: 'blur'},
            {min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur'}
          ]
        },
        uid:"",
        token:"",
        msg:""
      }
    },
    methods:{
      resetUserForm() {
        this.$refs.userFormRef.resetFields()
      },
      login(){
        this.$refs.userFormRef.validate(async valid => {
          if (!valid) return
          axios.get('/api/start/login/',
              {params: {username: this.userForm.username, password: this.userForm.password}}).then((res) => {
            this.msg = 'Successfully logged in!'
            this.uid = res.data.id
            this.token = res.data.token
          }).catch((error) => {
            this.msg = 'error'
            console.log(error);
          })
        })
      }
    }
  }
</script>

<style lang="less" scoped>
.login {
  // 宽450像素
  width: 450px;
  // 高300像素
  height: 200px;
  // 边框线1像素 实线
  border: 1px solid grey;
  // 圆角边框3像素
  border-radius: 3px;
  // 绝对定位
  position: absolute;
  // 距离左则50%
  left: 50%;
  // 上面距离30%
  top: 30%;
  // 进行位移，并且在横轴上位移-50%，纵轴也位移-50%
  transform: translate(-50%, -50%);
}
.user_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.user_btn {
  // 设置弹性布局
  display: flex;
  // 横轴上尾部对齐
  justify-content: flex-start;
}
.msg {
  // 宽450像素
  width: 450px;
  // 高300像素
  height: 150px;
  // 边框线1像素 实线
  border: 1px solid grey;
  // 圆角边框3像素
  border-radius: 3px;
  // 绝对定位
  position: absolute;
  // 距离左则50%
  left: 50%;
  // 上面距离30%
  top: 65%;
  // 进行位移，并且在横轴上位移-50%，纵轴也位移-50%
  transform: translate(-50%, -50%);
}
</style>
