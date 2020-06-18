<template>
  <div>
    <el-dialog
      title="修改报销信息"
      :visible.sync="dialogFormVisible">
      <el-form v-model="selectedBs" style="text-align: left" ref="dataForm">
        <el-form-item label="疾病名称" label-width="120px" prop="illness">
           <label>{{selectedBs.illness}}</label>
        </el-form-item>
        <el-form-item label="总费用" label-width="120px" prop="expenses">
          <el-input v-model="selectedBs.expenses" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="发票号" label-width="120px" prop="invoiceno">
          <el-input v-model="selectedBs.invoiceno" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="就诊时间" label-width="120px" prop="clinicaltime">
          <el-input v-model="selectedBs.clinicaltime" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="身份证" label-width="120px" prop="cardid">
          <el-input v-model="selectedBs.cardid" autocomplete="off"></el-input>
        </el-form-item>
         <el-form-item label="报销金额" label-width="120px" prop="amount">
          <el-input v-model="selectedBs.amount" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onSubmit(selectedBs)">确 定</el-button>
      </div>
    </el-dialog>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
         <el-breadcrumb-item>业务办理</el-breadcrumb-item>
        <el-breadcrumb-item>信息</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
     <searchx @onSearch="searchResult" ref="Searchx"></searchx>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="bss"
        stripe
        :default-sort = "{prop: 'id', order: 'ascending'}"
        style="width: 100%"
        :max-height="tableHeight">
        <el-table-column
          type="selection"
          width="55">
        </el-table-column>
        <el-table-column
          prop="id"
          label="id"
          sortable
          width="100">
        </el-table-column>
        <el-table-column
          prop="illness"
          label="疾病名称"
          fit>
        </el-table-column>
        <el-table-column
          prop="expenses"
          label="总费用"
          fit>
        </el-table-column>
        <el-table-column
          prop="invoiceno"
          label="发票号"
          fit>
        </el-table-column>
        <el-table-column
          prop="cardid"
          label="身份证"
          fit>
        </el-table-column>
         <el-table-column
          prop="amount"
          label="报销金额"
          fit>
        </el-table-column>
        <el-table-column
          prop="clinicaltime"
          label="就诊时间"
          show-overflow-tooltip
          fit>
        </el-table-column>
      </el-table>
      <div style="margin: 20px 0 20px 0;float: left">
        <el-button>取消选择</el-button>
        <el-button>批量删除</el-button>
      </div>
    </el-card>
  </div>
</template>

<script>
  import addBs from './addBs'
  import Searchx from './Searchx'
    export default {
      name: 'BsManagement',
      components: {addBs, Searchx},
      data () {
          return {
            bss: [],
            dialogFormVisible: false,
            selectedBs: []
          }
      },
      mounted () {
        this.listBss()
      },
      computed: {
        tableHeight () {
          return window.innerHeight - 320
        }
      },
      methods: {
        listBss () {
          var _this = this
          this.$axios.get('/admin/bss/list').then(resp => {
            console.log(resp)
            if (resp && resp.data.code === 200) {
              _this.bss = resp.data.result
            }
          })
        },
        onSubmit (bs) {
          this.$axios.put('/admin/bs/editBs', {
            illness: bs.illness,
            expenses: bs.expenses,
            invoiceno: bs.invoiceno,
            clinicaltime: bs.clinicaltime,
            cardid: bs.cardid,
            amount: bs.amount
          }).then(resp => {
            if (resp && resp.data.code === 200) {
              this.$alert('用户信息修改成功')
              this.dialogFormVisible = false
              // 修改角色后重新请求用户信息，实现视图更新
              this.listBss()
            } else {
              this.$alert(resp.data.message)
            }
          })
        },
        editBs (bs) {
          this.dialogFormVisible = true
          this.selectedBs = bs
        },
      deleteBs (id) {
        this.$confirm('此操作将永久删除, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$axios
              .delete('/admin/bs/deleteBs/' + id).then(resp => {
              if (resp && resp.data.code === 200) {
                this.listBss()
             } else {
                this.$alert(resp.data.message)
              }
            })
          }
        ).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          })
        })
      }
      }
    }
</script>

<style scoped>

</style>
