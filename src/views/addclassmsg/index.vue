<template>
  <div class="app-container">
    <el-form ref="form" :model="product" >
      <el-form-item label=" 分校 "  >
        <el-input v-model="formInline.school" style="width: 30%"  />
      </el-form-item>
      <el-form-item label="操作人">
        <el-input v-model="formInline.name" style="width: 30%"/>
      </el-form-item>
      <el-form-item label="操作时间">
        <el-input v-model="formInline.createTime" style="width: 30%"/>
      </el-form-item>

      <el-form-item>
      <el-button type="primary" v-model="formInline" @click="onSubmit">确定</el-button>
      </el-form-item>
    </el-form>

  </div>
</template>

<script>

  //发送post请求携带数据时需要导入该模块
  import Qs from 'qs'
  import { formatTime } from '../../utils'


  export default {
    data() {
      return {
        formInline: {
          school: '',
          name: '',
          createTime:''
        }
      }
    },
    methods: {
      onSubmit(){
        var vm =this;
        this.axios({
          method: 'POST',
          url: 'http://localhost:8080/api/addUser',
          data:this.formInline
        }).then(function(resp){
          // 添加成功后前往商品列表页面
            vm.$router.push("/classmsg")
          }
        );
      }
    }
  }
</script>


