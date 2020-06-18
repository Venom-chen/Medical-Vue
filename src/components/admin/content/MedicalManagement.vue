<template>
  <div>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
         <el-breadcrumb-item>报名管理</el-breadcrumb-item>
        <el-breadcrumb-item>面试信息</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <addMedical @onSubmit="listMedicals()"></addMedical>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="medicals"
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
          prop="zzjgbm"
          label="组织机构编号"
          fit>
        </el-table-column>
        <el-table-column
          prop="jgjb"
          label="机构级别"
          fit>
        </el-table-column>
        <el-table-column
          prop="jgmc"
          label="机构名称"
          fit>
        </el-table-column>
        <el-table-column
          prop="pzddlx"
          label="批准定点类型"
          fit>
        </el-table-column>
        <el-table-column
          prop="ssjjlx"
          label="所属经济类型"
          fit>
        </el-table-column>
        <el-table-column
          prop="wsjgxl"
          label="卫生机构小类"
          fit>
        </el-table-column>
        <el-table-column
          prop="zgdw"
          label="主管单位"
          fit>
        </el-table-column>
        <el-table-column
          prop="kysj"
          label="成立时间"
          fit>
        </el-table-column>
         <el-table-column
          prop="frdb"
          label="法人"
          fit>
        </el-table-column>
         <el-table-column
          prop="zczj"
          label="注册资金"
          show-overflow-tooltip
          fit>
        </el-table-column>
        <el-table-column
          label="操作"
          width="120">
          <template slot-scope="scope">
            <el-button
              @click.native.prevent="deleteMedical(scope.row.id)"
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
   import addMedical from './addMedical'
    export default {
      name: 'MedicalManagement',
      components: {addMedical},
      data () {
          return {
            medicals: [],
            dialogFormVisible: false,
            selectedMedical: []
          }
      },
      mounted () {
        this.listMedicals()
      },
      computed: {
        tableHeight () {
          return window.innerHeight - 320
        }
      },
      methods: {
        listMedicals () {
          var _this = this
          this.$axios.get('/admin/medical/list').then(resp => {
            if (resp && resp.data.code === 200) {
              _this.medicals = resp.data.result
            }
          })
        },
        onSubmit (medical) {
          this.$axios.put('/admin/medical/editMedical', {
            zzjgbm: '',
            jgjb: '',
            jgmc: '',
            pzddlx: '',
            ssjjlx: '',
            wsjgxl: '',
            zgdw: '',
            kysj: '',
            frdb: '',
            zczj: ''
          }).then(resp => {
            if (resp && resp.data.code === 200) {
              this.$alert('修改成功')
              this.dialogFormVisible = false
              // 修改角色后重新请求用户信息，实现视图更新
              this.listMedicals()
            } else {
              this.$alert(resp.data.message)
            }
          })
        },
        editMedical (medical) {
          this.dialogFormVisible = true
          this.selectedMedical = medical
        },
      deleteMedical (id) {
        this.$confirm('此操作将永久删除, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$axios
              .delete('/admin/medical/deleteMedical/' + id).then(resp => {
              if (resp && resp.data.code === 200) {
                this.listMedicals()
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
