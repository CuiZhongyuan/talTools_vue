<!--suppress ALL -->
<template>
  <div class="app-container">
    <el-form ref="form" :model="form" label-width="70px">
      <el-form-item label="记录ID">
        <el-input v-model="form.id" />
      </el-form-item>
      <el-form-item label="分校">
        <el-input v-model="form.school" />
      </el-form-item>
      <el-form-item label="操作人">
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item label="操作时间">
        <el-input v-model="form.createTime" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-model="form" @click="editUser">确定</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        id: '',
        school: '',
        name: '',
        createTime: '',
        testResult: '',
        remark: ''
      }
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      // eslint-disable-next-line no-unused-vars
      var id = this.$route.params.id
      var vm = this
      this.axios({
        method: 'GET',
        url: 'http://127.0.0.1:8080/api/queryId?id=' + id
      }).then(function(resp) {
          vm.form.id=resp.data.list[0].id,
          vm.form.name = resp.data.list[0].name,
          vm.form.school= resp.data.list[0].school,
          vm.form.createTime=resp.data.list[0].createTime
      })
    },
    editUser() {
      // eslint-disable-next-line no-unused-vars
      var vm = this
      this.axios({
        method: 'POST',
        url: 'http://127.0.0.1:8080/api/editUser',
        data: vm.form
      }).then(function (resp) {
        vm.$router.push("/classmsg")
        // eslint-disable-next-line eqeqeq
        if (resp.data == 'success') {
          // 弹框
          vm.$message({
            message: '编辑成功',
            type: 'success'
          })
        } else {
          vm.$message.error('编辑失败')
        }
      })
    },
    onCancel() {
      this.$message({
        message: 'cancel!',
        type: 'warning'
      })
    }
  }
}
</script>

<style scoped>
.line{
  text-align: center;
}
</style>

