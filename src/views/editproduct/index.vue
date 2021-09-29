<template>
  <div class="app-container">
    <el-form ref="form" :model="product" label-width="120px">
      <el-form-item label="商品名称">
        <el-input v-model="product.name"/>
      </el-form-item>
      <el-form-item label="商品价格">
        <el-input v-model="product.price"/>
      </el-form-item>

      <el-form-item>
      <el-button type="primary" @click="onSubmit">编辑</el-button>
      </el-form-item>

    </el-form>

  </div>
</template>

<script>

  //发送post请求携带数据时需要导入该模块
  import Qs from 'qs'


  export default {
    data() {
      return {
        product: {
          'id': 4,
          'name': 'HUAWEI P30',
          'images': 'aaaa',
          'price': 4288,
          'salePrice': 222,
          'salePoint': 'sdf',
          'typeId': 1,
          'typeName': 'sss',
          'flag': true,
          'createTime': 'Jun 5, 2019 12:10:29 AM',
          'updateTime': 'Jun 21, 2019 12:10:34 AM',
          'createUser': 1,
          'updateUser': 1
        }
      }
    },
    created() {
      this.fatchDataById()
    },
    methods: {

      fatchDataById() {
        var id = this.$route.params.id
        var vm = this
        this.axios({

            method: 'GET',
            url: 'http://localhost:8090/product/getProductById?id=' + id

          }
        ).then(function(resp) {

          vm.product = resp.data

        })

      },
      onSubmit(){
        var vm = this;
        this.axios({
          method: 'POST',
          url: 'http://localhost:8090/product/editproduct',
          //=========解决POST请求无法携带数据的问题===========
          transformRequest: [function (data) {
            return Qs.stringify(data)
          }],
          //===============================
          data:vm.product
        }).then(function(resp){
            vm.$router.push("/products")

          }
        );
      }





      // onSubmit() {
      //   this.$message('submit!')
      // },
      // onCancel() {
      //   this.$message({
      //     message: 'cancel!',
      //     type: 'warning'
      //   })
      // }
    }
  }
</script>

<style scoped>
  .line {
    text-align: center;
  }
</style>

