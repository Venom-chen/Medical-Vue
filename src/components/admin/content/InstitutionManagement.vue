<template>
  <div>
    <el-dialog
      title="修改农合机构信息"
      :visible.sync="dialogFormVisible">
      <el-form v-model="selectedIns" style="text-align: left" ref="dataForm">
        <el-form-item label="所属地区" label-width="120px" prop="areacode">
          <label>{{selectedIns.areacode}}</label>
        </el-form-item>
        <el-form-item label="经办机构编码" label-width="120px" prop="agencode">
          <el-input v-model="selectedIns.agencode" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="经办机构编码名称" label-width="120px" prop="agenname">
          <el-input v-model="selectedIns.agenname" autocomplete="off" ></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onSubmit(selectedIns)">确 定</el-button>
      </div>
    </el-dialog>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
         <el-breadcrumb-item>报名管理</el-breadcrumb-item>
        <el-breadcrumb-item>报名信息</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <addInstitution @onSubmit="listInss()"></addInstitution>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="inss"
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
          prop="areacode"
          label="所属地区"
          fit>
        </el-table-column>
        <el-table-column
          prop="agencode"
          label="经办机构编码"
          fit>
        </el-table-column>
        <el-table-column
          prop="agenname"
          label="经办机构编码名称"
           show-overflow-tooltip
          fit>
        </el-table-column>
        <el-table-column
          label="操作"
          width="120">
          <template slot-scope="scope">
            <el-button
              @click="editIns(scope.row)"
              type="text"
              size="small">
              编辑
            </el-button>
            <el-button
              @click.native.prevent="deleteIns(scope.row.id)"
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
  import addInstitution from './addInstitution'
    export default {
      name: 'InstitutionManagement',
      components: {addInstitution},
      data () {
          return {
            inss: [],
            dialogFormVisible: false,
            selectedIns: []
          }
      },
      mounted () {
        this.listInss()
      },
      computed: {
        tableHeight () {
          return window.innerHeight - 320
        }
      },
      methods: {
        listInss () {
          var _this = this
          this.$axios.get('/admin/institution/list').then(resp => {
            if (resp && resp.data.code === 200) {
              _this.inss = resp.data.result
            }
          })
        },
        onSubmit (ins) {
          this.$axios.put('/admin/institution/editInstitution', {
            areacode: ins.areacode,
            agencode: ins.agencode,
            agenname: ins.agenname
          }).then(resp => {
            if (resp && resp.data.code === 200) {
              this.$alert('用户信息修改成功')
              this.dialogFormVisible = false
              // 修改角色后重新请求用户信息，实现视图更新
              this.listInss()
            } else {
              this.$alert(resp.data.message)
            }
          })
        },
        editIns (ins) {
          this.dialogFormVisible = true
          this.selectedIns = ins
        },
      deleteIns (id) {
        this.$confirm('此操作将永久删除, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$axios
              .delete('/admin/institution/deleteInstitution/' + id).then(resp => {
              if (resp && resp.data.code === 200) {
                this.listInss()
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
