<template>
  <div>
    <el-dialog
      title="修改行政信息"
      :visible.sync="dialogFormVisible">
      <el-form v-model="selectedArea" style="text-align: left" ref="dataForm">
        <el-form-item label="行政区域编号" label-width="120px" prop="areacode">
          <label>{{selectedArea.areacode}}</label>
        </el-form-item>
        <el-form-item label="行政区域名称" label-width="120px" prop="areaname">
          <el-input v-model="selectedArea.areaname" autocomplete="off"></el-input>
        </el-form-item>
         <el-form-item label="级别" label-width="120px" prop="grade">
          <el-input v-model="selectedArea.grade" autocomplete="off"></el-input>
        </el-form-item>
        </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onSubmit(selectedArea)">确 定</el-button>
      </div>
    </el-dialog>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
         <el-breadcrumb-item>信息管理</el-breadcrumb-item>
        <el-breadcrumb-item>行政信息</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <addArea @onSubmit="listAreas()"></addArea>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="areas"
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
          label="行政区域编号"
          fit>
        </el-table-column>
         <el-table-column
          prop="grade"
          label="级别"
          fit>
        </el-table-column>
        <el-table-column
          prop="areaname"
          label="行政区域名称"
          show-overflow-tooltip
          fit>
        </el-table-column>
        <el-table-column
          label="操作"
          width="120">
          <template slot-scope="scope">
            <el-button
              @click="editArea(scope.row)"
              type="text"
              size="small">
              编辑
            </el-button>
            <el-button
              @click.native.prevent="deleteArea(scope.row.id)"
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
  import addArea from './addArea'
    export default {
      name: 'AreaManagement',
      components: {addArea},
      data () {
          return {
            areas: [],
            dialogFormVisible: false,
            selectedArea: []
          }
      },
      mounted () {
        this.listAreas()
      },
      computed: {
        tableHeight () {
          return window.innerHeight - 320
        }
      },
      methods: {
        listAreas () {
          var _this = this
          this.$axios.get('/admin/area/list').then(resp => {
            console.log(resp)
            if (resp && resp.data.code === 200) {
              _this.areas = resp.data.result
            }
          })
        },
        onSubmit (area) {
          this.$axios.put('/admin/area/editArea', {
            areacode: area.areacode,
            areaname: area.areaname,
            grade: area.grade
          }).then(resp => {
            if (resp && resp.data.code === 200) {
              this.$alert('用户信息修改成功')
              this.dialogFormVisible = false
              // 修改角色后重新请求用户信息，实现视图更新
              this.listAreas()
            } else {
              this.$alert(resp.data.message)
            }
          })
        },
        editArea (area) {
          this.dialogFormVisible = true
          this.selectedArea = area
        },
      deleteArea (id) {
        this.$confirm('此操作将永久删除, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$axios
              .delete('/admin/area/deleteArea/' + id).then(resp => {
              if (resp && resp.data.code === 200) {
                this.listAreas()
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
