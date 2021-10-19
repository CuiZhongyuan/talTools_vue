<template>
  <div class="app-container">

    <div align="left">
      <el-form :inline="true" :model="formInline" class="demo-form-inline">
        <el-form-item label="分校">
          <el-select v-model="formInline.school" placeholder="分校">
            <el-option label="北京" value="北京" />
            <el-option label="郑州" value="郑州" />
          </el-select>
        </el-form-item>
        <el-form-item label="操作人">
          <el-input v-model="formInline.name" placeholder="操作人" />
        </el-form-item>
        <el-form-item label="操作时间">
          <el-input v-model="formInline.createTime" placeholder="操作时间" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="queryResult">查询</el-button>
          <el-button type="primary" @click="addResult">新增</el-button>
        </el-form-item>
      </el-form>
    </div>

    <el-table :data="list" border fit highlight-current-row>
      <el-table-column align="center" label="序号" width="120">
        <template slot-scope="scope">
          {{ scope.$index+1 }}
        </template>
      </el-table-column>

      <el-table-column label="记录ID" width="100">
        <template slot-scope="scope">
          {{ scope.row.id }}
        </template>
      </el-table-column>

      <el-table-column label="分校" width="100">
        <template slot-scope="scope">
          {{ scope.row.school }}
        </template>
      </el-table-column>

      <el-table-column label="操作人" width="150">
        <template slot-scope="scope">
          {{ scope.row.name }}
        </template>
      </el-table-column>

      <el-table-column label="操作时间" width="210">
        <template slot-scope="scope">
          {{ scope.row.createTime }}
        </template>
      </el-table-column>

      <el-table-column label="查看测试结果" width="250">
        <template slot-scope="scope">
          {{ scope.row.testResult }}
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center" width="200" class-name="small-padding fixed-width">
        <template slot-scope="scope">
          <el-button size="mini" @click="editUser(scope.row.id)">编辑</el-button>
          <el-button size="mini" type="danger" @click="deleteUser(scope.row.id)">删除</el-button>
        </template>
      </el-table-column>

    </el-table>
    <!--pagination分页-->
    <pagination v-show="total>0" :background="bkcolor" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit" @pagination="getList" />

  </div>
</template>

<script>

import Pagination from '@/components/Pagination'

export default {
  components: { Pagination },
  data() {
    return {
      bkcolor: false,
      total: 0,
      listQuery: {
        page: 1,
        limit: 10
      },
      list: null,
      formInline: {
        id: '',
        school: '北京',
        name: '',
        createTime: '',
        testResult: '',
        remark: ''
      }
    }
  },
  // vue生命周期钩子函数
  created() {
    this.getList()
  },
  methods: {
    queryResult() {
      // 因为axios内部的then内部的this是指的axios对象，而不是当前vue的对象，因此把当前Vue的对象用vm声明，这样this就不会冲突
      var vm = this
      this.axios({
        method: 'GET',
        url: 'http://localhost:8080/api/queryList?school=' + vm.formInline.school + '&&name=' + vm.formInline.name + '&&createTime=' + vm.formInline.createTime
      }).then(function(response) {
        // 得到一个PageInfo对象
        vm.total = response.data.total// 将PageInfo中的total赋给vm的total
        vm.list = response.data.list// 当前页显示的所有数据
      })
    },
    onSubmit() {
      console.log('submit!')
    },
    getList() {
      var vm = this
      this.axios({
        method: 'GET',
        url: 'http://localhost:8080/api/getPage?pageNum=' + vm.listQuery.page + '&pageSize=' + vm.listQuery.limit
      }).then(function(response) {
        // 得到一个PageInfo对象
        vm.total = response.data.total// 将PageInfo中的total赋给vm的total
        vm.list = response.data.list// 当前页显示的所有数据
      })
    },
    addResult() {
      this.$router.push('/addclassmsg/index')
    },
    editUser(id) {
      this.$router.push('/editUser/index/' + id)
    },
    deleteUser(id) {
      var vm = this
      this.axios({
        method: 'GET',
        url: 'http://localhost:8080/api/deleteUser?id=' + id
      }).then(function(resp) {
        // eslint-disable-next-line eqeqeq
        if (resp.data == 'success') {
          // 弹框
          vm.$message({
            message: '删除成功',
            type: 'success'
          })
          // 刷新当前页
          location.reload()
        } else {
          vm.$message.error('删除失败')
        }
        // eslint-disable-next-line handle-callback-err
      }).catch(function(error) {
        // 弹框
        vm.$message.error('删除失败')
      })
    }

  }
}
</script>

