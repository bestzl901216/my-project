<template>
  <div class="goods">
    <h1>商品页面</h1>
    <el-button @click="dialogFormVisible = true" type="primary" size="small">新增</el-button>
    <el-table :data="tableData" stripe style="width: 100%">
      <el-table-column prop="date" label="日期" width="180" sortable/>
      <el-table-column prop="name" label="姓名" width="180" sortable/>
      <el-table-column prop="address" label="地址"/>
    </el-table>
    <el-pagination
      background
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="3"
      :page-sizes="[100, 200, 300, 400]"
      :page-size="100"
      layout="total, sizes, prev, pager, next, jumper"
      :total="1000">
    </el-pagination>
    <el-dialog title="新增商品信息" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="商品名称">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="商品规格">
          <el-input v-model="form.specification"></el-input>
        </el-form-item>
        <el-form-item label="生产厂商">
          <el-input v-model="form.manufacturer"></el-input>
        </el-form-item>
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="addGoods(form)">确 定</el-button>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
import api from '@/api'
export default {
  name: 'Goods',
  data () {
    return {
      dialogFormVisible: false,
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-04',
        name: '周小虎',
        address: '上海市普陀区金沙江路 1517 弄'
      }, {
        date: '2016-05-01',
        name: '李小虎',
        address: '上海市普陀区金沙江路 1519 弄'
      }, {
        date: '2016-05-03',
        name: '蒋小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      }],
      form: {
        name: '',
        specification: '',
        manufacturer: ''
      }
    }
  },
  methods: {
    addGoods (data) {
      console.info('新增商品')
      this.$http.post(api.goods.creat, data)
    },
    handleSizeChange (data) {
      console.info('size变化')
    },
    handleCurrentChange (data) {
      console.info('current变化')
    }
  }
}
</script>

<style scoped>

</style>
