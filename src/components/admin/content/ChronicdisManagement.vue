<template>
  <div>
    <el-dialog
      title="修改分类信息"
      :visible.sync="dialogFormVisible">
      <el-form v-model="selectedChronicdi" style="text-align: left" ref="dataForm">
        <el-form-item label="疾病编号" label-width="120px" prop="illcode">
          <label>{{selectedChronicdi.illcode}}</label>
        </el-form-item>
        <el-form-item label="疾病名称" label-width="120px" prop="illname">
          <el-input v-model="selectedChronicdi.illname" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="拼音码" label-width="120px" prop="pycode">
          <el-input v-model="selectedChronicdi.pycode" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="五笔码" label-width="120px" prop="wbcode">
          <el-input v-model="selectedChronicdi.wbcode" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onSubmit(selectedChronicdi)">确 定</el-button>
      </div>
    </el-dialog>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
         <el-breadcrumb-item>信息管理</el-breadcrumb-item>
        <el-breadcrumb-item>报修信息</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <addChronicdis @onSubmit="listChronicdis()"></addChronicdis>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="chronicdis"
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
          prop="illcode"
          label="疾病编号"
          fit>
        </el-table-column>
        <el-table-column
          prop="illname"
          label="疾病名称"
          fit>
        </el-table-column>
        <el-table-column
          prop="pycode"
          label="拼音码"
          fit>
        </el-table-column>
         <el-table-column
          prop="wbcode"
          label="五笔码"
          show-overflow-tooltip
          fit>
        </el-table-column>
        <el-table-column
          label="操作"
          width="120">
          <template slot-scope="scope">
            <el-button
              @click="editChronicdi(scope.row)"
              type="text"
              size="small">
              编辑
            </el-button>
            <el-button
              @click.native.prevent="deleteChronicdi(scope.row.id)"
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
  import addChronicdis from './addChronicdis'
    export default {
      name: 'ChronicdisManagement',
      components: {addChronicdis},
      data () {
          return {
            chronicdis: [],
            dialogFormVisible: false,
            selectedChronicdi: []
          }
      },
      mounted () {
        this.listChronicdis()
      },
      computed: {
        tableHeight () {
          return window.innerHeight - 320
        }
      },
      methods: {
        listChronicdis () {
          var _this = this
          this.$axios.get('/admin/chronicdis/list').then(resp => {
            if (resp && resp.data.code === 200) {
              _this.chronicdis = resp.data.result
            }
          })
        },
        onSubmit (chronicdi) {
          this.$axios.put('/admin/chronicdis/editChronicdis', {
            illcode: chronicdi.illcode,
            illname: chronicdi.illname,
            pycode: chronicdi.pycode,
            wbcode: chronicdi.wbcode
          }).then(resp => {
            if (resp && resp.data.code === 200) {
              this.$alert('用户信息修改成功')
              this.dialogFormVisible = false
              // 修改角色后重新请求用户信息，实现视图更新
              this.listChronicdis()
            } else {
              this.$alert(resp.data.message)
            }
          })
        },
        editChronicdi (chronicdi) {
          this.dialogFormVisible = true
          this.selectedChronicdi = chronicdi
        },
      deleteChronicdi (id) {
        this.$confirm('此操作将永久删除, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$axios
              .delete('/admin/chronicdis/deleteChronicdis/' + id).then(resp => {
              if (resp && resp.data.code === 200) {
                this.listChronicdis()
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
