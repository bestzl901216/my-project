<template>
  <div class="goods">
    <h1>商品页面</h1>
    <div class="search">
      <el-form :model="searchParams" ref="searchParams" :inline="true">
        <el-form-item label="ID：" prop="id">
          <el-input v-model="searchParams.id"></el-input>
        </el-form-item>
        <el-form-item label="名称：" prop="name">
          <el-input v-model="searchParams.name"></el-input>
        </el-form-item>
        <el-form-item label="规格：" prop="specification">
          <el-input v-model="searchParams.specification"></el-input>
        </el-form-item>
        <el-form-item label="厂商：" prop="manufacturer">
          <el-input v-model="searchParams.manufacturer"></el-input>
        </el-form-item>
        <el-form-item label="状态：" prop="status">
          <el-select v-model="searchParams.status" placeholder="请选择">
            <el-option
              v-for="item in goodsStatusDict"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-button type="primary" @click="doSearch(searchParams)">确 定</el-button>
        <el-button @click="reset('searchParams')">重 置</el-button>
      </el-form>
    </div>
    <el-button @click="dialogFormVisible = true" type="primary" size="small">新增</el-button>
    <el-table :data="tableData" stripe style="width: 100%">
      <el-table-column prop="id" label="ID"/>
      <el-table-column prop="name" label="名称"/>
      <el-table-column prop="specification" label="规格"/>
      <el-table-column prop="manufacturer" label="厂商"/>
      <el-table-column prop="status" label="状态" :formatter="goodsStatusFormatter"/>
    </el-table>
    <el-pagination
      background
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :page-sizes="[1, 5, 10, 20]"
      :page-size="this.tableParams.size"
      :current-page="this.tableParams.page"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total">
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
  created () {
    this.getGoodsStatusDict()
  },
  data () {
    return {
      searchParams: {
      },
      tableParams: {
        id: null,
        name: null,
        specification: null,
        manufacturer: null,
        status: null,
        page: 1,
        size: 5
      },
      total: 0,
      tableData: [],
      form: {
        name: '',
        specification: '',
        manufacturer: ''
      },
      goodsStatusDict: null,
      dialogFormVisible: false
    }
  },
  watch: {
    tableParams: {
      handler (newValue, oldValue) {
        this.listGoodsByPage(newValue)
      },
      deep: true,
      immediate: true
    }
  },
  methods: {
    doSearch (data) {
      this.tableParams = Object.assign(this.tableParams, data)
      console.info('data', data)
      console.info('params', this.tableParams)
    },
    reset (formName) {
      this.$refs[formName].resetFields()
    },
    handleSizeChange (data) {
      this.tableParams.page = 1
      this.tableParams.size = data
    },
    handleCurrentChange (data) {
      this.tableParams.page = data
    },
    goodsStatusFormatter (row, column, cellValue, index) {
      if (this.goodsStatusDict !== null) {
        for (let i = 0; i < this.goodsStatusDict.length; i++) {
          if (cellValue === this.goodsStatusDict[i].value) {
            return this.goodsStatusDict[i].label
          }
        }
      }
    },
    listGoodsByPage (params) {
      this.$http.get(api.goods, {params: params}).then(res => {
        this.tableData = res.body.rows
        this.total = res.body.total
      })
    },
    getGoodsStatusDict () {
      this.$http.get(api.goodsStatusDict).then(res => {
        this.goodsStatusDict = res.body
      })
    },
    addGoods (data) {
      this.$http.post(api.goods, data)
    }
  }
}
</script>

<style scoped>

</style>
