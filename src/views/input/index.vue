<template>
  <div class="app-container">
    <div class="tag-group">
      <span class="tag-group__title" />
      <el-tag
        v-for="item in items"
        :key="item.label"
        :type="item.type"
        effect="dark"
      >
        {{ item.label }}
      </el-tag>
    </div>
    <el-form ref="form" :model="onSubmit">
      <el-form-item>
        <el-input v-model="textarea" type="textarea" :rows="15" placeholder="请输入内容" style="width: 80%" />
      </el-form-item>
      <el-form-item>
        <el-button v-model="textarea" type="primary" @click="onSubmit">确定</el-button>
      </el-form-item>
      <el-form-item>
        <el-input v-model="groovy.outParams" type="textarea" :rows="3" placeholder="groovy输出结果" style="width: 80%" />
      </el-form-item>
    </el-form>

  </div>
</template>

<script>

export default {
  data() {
    return {
      items: [
        { type: '', label: '输入Groovy脚本' }
      ],
      formInline: {
        school: '',
        name: '',
        createTime: ''
      },
      textarea: '',
      groovy: {
        outParams: ''
      }
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
        // JSON.stringify(resp.data.outParams)，object对象转json字符串
        vm.groovy.outParams = JSON.stringify(resp.data.outParams)
        console.log(resp)
      }
      )
    }
  }
}
</script>

