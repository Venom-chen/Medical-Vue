<template>
  <div>
    <el-dialog
      title="修改封顶线信息"
      :visible.sync="dialogFormVisible">
      <el-form v-model="selectedPolicy" style="text-align: left" ref="dataForm">
        <el-form-item label="ID" label-width="120px" prop="id">
            <label>{{selectedPolicy.id}}</label>
        </el-form-item>
        <el-form-item label="执行年度" label-width="120px" prop="runyear">
          <el-input v-model="selectedPolicy.runyear" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="封顶线" label-width="120px" prop="maxline">
          <el-input v-model="selectedPolicy.maxline" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="状态" label-width="120px" prop="status">
        <el-select v-model="selectedPolicy.status" autocomplete="off">
          <el-option label="启用" value="1"></el-option>
          <el-option label="禁用" value="0"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item label="备注" label-width="120px" prop="remark">
          <el-input v-model="selectedPolicy.remark" autocomplete="off"></el-input>
        </el-form-item>
         <el-form-item label="报销比例" label-width="120px" prop="rate">
          <el-input v-model="selectedPolicy.rate" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onSubmit(selectedPolicy)">确 定</el-button>
      </div>
    </el-dialog>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
         <el-breadcrumb-item>信息管理</el-breadcrumb-item>
        <el-breadcrumb-item>封顶线信息</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <addPolicy @onSubmit="listPolicys()"></addPolicy>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="policys"
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
          prop="runyear"
          label="执行年度"
          fit>
        </el-table-column>
        <el-table-column
          prop="maxline"
          label="封顶线"
          fit>
        </el-table-column>
        <el-table-column
          prop="remark"
          label="备注"
          fit>
        </el-table-column>
         <el-table-column
          prop="rate"
          label="报销比例"
          fit>
        </el-table-column>
        <el-table-column
           prop="status"
          label="状态"
          fit>
        </el-table-column>
        <el-table-column
          label="操作"
          width="120">
          <template slot-scope="scope">
            <el-button
              @click="editPolicy(scope.row)"
              type="text"
              size="small">
              编辑
            </el-button>
            <el-button
              @click.native.prevent="deletePolicy(scope.row.id)"
              type="text"
              size="small">
              移除
            </el-button>
          </template>
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
  import addPolicy from './addPolicy'
    export default {
      name: 'PolicyManagement',
      components: {addPolicy},
      data () {
          return {
            policys: [],
            dialogFormVisible: false,
            selectedPolicy: []
          }
      },
      mounted () {
        this.listPolicys()
      },
      computed: {
        tableHeight () {
          return window.innerHeight - 320
        }
      },
      methods: {
        listPolicys () {
          var _this = this
          this.$axios.get('/admin/policy/list').then(resp => {
            console.log(resp)
            if (resp && resp.data.code === 200) {
              _this.policys = resp.data.result
            }
          })
        },
        onSubmit (policy) {
          this.$axios.put('/admin/policy/editPolicy', {
            id: policy.id,
            runyear: policy.runyear,
            maxline: policy.maxline,
            status: policy.status,
            remark: policy.remark,
            rate: policy.rate
          }).then(resp => {
            if (resp && resp.data.code === 200) {
              this.$alert('信息修改成功')
              this.dialogFormVisible = false
              // 修改角色后重新请求用户信息，实现视图更新
              this.listPolicys()
            } else {
              this.$alert(resp.data.message)
            }
          })
        },
        editPolicy (policy) {
          this.dialogFormVisible = true
          this.selectedPolicy = policy
        },
      deletePolicy (id) {
        this.$confirm('此操作将永久删除, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$axios
              .delete('/admin/policy/deletePolicy/' + id).then(resp => {
              if (resp && resp.data.code === 200) {
                this.listPolicys()
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
