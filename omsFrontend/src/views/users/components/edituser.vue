<template xmlns="http://www.w3.org/1999/html">
  <el-form :model="rowdata" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
    <el-form-item label="用户名" prop="username">
      <el-input v-model="rowdata.username"></el-input>
    </el-form-item>
    <el-form-item label="Email" prop="email">
      <el-input v-model="rowdata.email"></el-input>
    </el-form-item>
    <el-form-item label="Skype" prop="email">
      <el-input v-model="rowdata.skype"></el-input>
    </el-form-item>
    <el-form-item label="用户分组" prop="group">
      <el-select v-model="rowdata.group" multiple placeholder="请选择用户组">
        <el-option v-for="item in groups" :key="item.name" :value="item.name"></el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="是否激活" prop="is_active">
      <el-switch on-text="oo" off-text="xx" v-model="rowdata.is_active"></el-switch>
    </el-form-item>
    <el-form-item label="角色" prop="group">
      <el-select v-model="rowdata.roles" placeholder="请选择用户角色">
        <el-option v-for="item in roles" :key="item.name" :value="item.name"></el-option>
      </el-select>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="postForm('ruleForm')">提交</el-button>
    </el-form-item>
  </el-form>
</template>
<script>
import { patchUser, getGroup, getRole } from 'api/user'

export default {
  components: {},

  props: ['rowdata'],
  data() {
    return {
      changePass: false,
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        email: [
          { required: true, type: 'email', message: '请输入正确的Email地址', trigger: 'blur' }
        ],
        skype: [
          { required: true, message: '请输入正确的Skype地址', trigger: 'blur' }
        ],
        group: [
          { required: true, type: 'array', message: '请选择用户分组', trigger: 'change' }
        ],
        roles: [
          { required: true, message: '请选择用户角色', trigger: 'blur' }
        ]
      },
      groups: '',
      roles: ''
    }
  },

  created() {
    this.getGroups()
    this.getRoles()
  },
  methods: {
    postForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.rowdata.password = 'qwert@12345'
          patchUser(this.rowdata.id, this.rowdata).then(response => {
            if (response.statusText === 'ok') {
              this.$message({
                type: 'success',
                message: '恭喜你，更新成功'
              })
            }
          }).catch(error => {
            this.$message.error('更新失败')
            console.log(error)
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
      this.$emit('DialogStatus', false)
    },

    getHosts(data) {
      this.rowdata.hosts = data
    },
    getGroups() {
      getGroup().then(response => {
        this.groups = response.data.results
      })
    },
    getRoles() {
      getRole().then(response => {
        this.roles = response.data.results
      })
    },
    setPasswd() {
      this.rowdata.password = Math.random().toString(35).slice(2)
      console.log(this.rowdata.password)
    }
  }
}
</script>

<style lang='scss'>

</style>
