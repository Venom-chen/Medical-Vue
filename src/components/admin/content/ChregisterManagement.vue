<template>
  <div>
    <search @onSearch="searchResult" ref="Search"></search>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
         <el-breadcrumb-item>信息管理</el-breadcrumb-item>
        <el-breadcrumb-item>参合信息</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="persons"
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
          prop="persname"
          label="成员姓名"
          show-overflow-tooltip
          fit>
        </el-table-column>
        <el-table-column
          label="操作"
          width="120">
            <template slot-scope="scope">
            <el-button
              @click.native.prevent="editPerson(scope.row.persname)"
              type="text"
              size="small">
              参合
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
    import Search from './Search'
    export default {
      name: 'ChregisterManagement',
      components: {Search},
      data () {
          return {
            persons: [],
            dialogFormVisible: false,
            selectedPerson: []
          }
      },
      mounted () {
        this.listPersons()
      },
      computed: {
        tableHeight () {
          return window.innerHeight - 320
        }
      },
      methods: {
        searchResult () {
        var _this = this
        this.$axios
          .get('/search?persname=' + this.$refs.Search.persname, {
          }).then(resp => {
          if (resp && resp.data.code === 200) {
            _this.persons = resp.data.result
          }
        })
      },
        editPerson (persname) {
          this.$confirm('是否参合?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$axios
              .delete('/admin/person/editPersons/' + persname).then(resp => {
              if (resp && resp.data.code === 200) {
                this.$alert('参合成功')
              } else {
              this.$alert(resp.data.message)
            }
            })
          }
        ).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消参合'
          })
        })
        },
      deletePerson (id) {
        this.$confirm('此操作将永久删除, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$axios
              .delete('/admin/person/deletePerson/' + id).then(resp => {
              if (resp && resp.data.code === 200) {
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
