<template>
  <div class="app-container">
    <el-form ref="form" :model="onSubmit" >
      <el-form-item>
        <el-input type="textarea" :rows="9" placeholder="请输入内容" v-model="textarea" style="width: 80%">
        </el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-model="textarea" @click="onSubmit">确定</el-button>
      </el-form-item>
      <el-form-item  >
        <el-input type="textarea" :rows="3" placeholder="groovy输出结果" v-model="groovy" style="width: 80%">
        </el-input>
      </el-form-item>
    </el-form>

  </div>
</template>

<script>

export default {
  data() {
    return {
      formInline: {
        school: '',
        name: '',
        createTime: ''
      },
      textarea: '',
      groovy: ''
    }
  },
  methods: {
    onSubmit() {
      // eslint-disable-next-line no-unused-vars
      var vm = this
      // groovy脚本实现
      this.axios({
        method: 'POST',
        headers: {
          'Content-Type': 'text/plain'
        },
        url: 'http://localhost:8080/api/groovy',
        data: this.textarea
      }).then(function(resp) {
        vm.groovy = resp.data
        console.log(resp)
      }
      )
    }
  }
}
</script>

