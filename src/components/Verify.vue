<template>
  <div class="verify">
    <el-form ref="userFormRef" :model="userForm" :rules="userFormRules" label-width="0px" class="user_form">
      <!-- 用户id -->
      <el-form-item prop="id">
        <el-input
            v-model="userForm.id" placeholder="id"
        >
        </el-input>
      </el-form-item>
      <!-- token -->
      <el-form-item prop="token">
        <el-input
            v-model="userForm.token" type="token" placeholder="token"
        >
        </el-input>
      </el-form-item>
      <!-- 按钮区域 -->
      <el-row justify="center">
        <el-form-item class="user_btn">
          <el-button type="primary" @click="verify()">验证</el-button>
          <el-button type="info" @click="resetUserForm">重置</el-button>
        </el-form-item>
      </el-row>
    </el-form>
  </div>
  <div class="msg">
    <p>{{msg}}</p>
  </div>
</template>

<script>
const axios=require('axios').default
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Verify",
  data(){
    return {
      // 这是表单的数据绑定对象
      userForm: {
        id: '',
        token: ''
      },
      // 这是表单的验证规则对象
      userFormRules: {
        id: [
          {required: true, message: '不能为空', trigger: 'blur'},
          {min: 1, max: 10, message: '长度在 1 到 10 个字符', trigger: 'blur'}
        ],
        token: [
          {required: true, message: '不能为空', trigger: 'blur'},
          {min: 32, max: 32, message: '长度 32 个字符', trigger: 'blur'}
        ]
      },
      msg: "",
    }
  },
  methods:{
    resetUserForm() {
      this.$refs.userFormRef.resetFields()
    },
    verify(){
      this.$refs.userFormRef.validate(async valid => {
        if (!valid) return
        axios.get('/api/start/verify/',
            {params: {id: this.userForm.id, token: this.userForm.token}}).then((res) => {
          this.msg = 'User credentials are correct.'
          this.id = res.data.id
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
.verify {
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
  height: 100px;
  // 边框线1像素 实线
  border: 1px solid grey;
  // 圆角边框3像素
  border-radius: 3px;
  // 绝对定位
  position: absolute;
  // 距离左则50%
  left: 50%;
  // 上面距离30%
  top: 60%;
  // 进行位移，并且在横轴上位移-50%，纵轴也位移-50%
  transform: translate(-50%, -50%);
}
</style>.