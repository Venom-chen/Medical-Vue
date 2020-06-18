<template>
  <div>
    <el-dialog
      title="修改慢病证信息"
      :visible.sync="dialogFormVisible">
      <el-form v-model="selectedChrmanagement" style="text-align: left" ref="dataForm">
        <el-form-item label="慢病证号" label-width="120px" prop="chronid">
          <label>{{selectedChrmanagement.chronid}}</label>
        </el-form-item>
        <el-form-item label="农合证号" label-width="120px" prop="ruralcard">
          <el-input v-model="selectedChrmanagement.ruralcard" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="身份证号" label-width="120px" prop="cardid">
          <el-input v-model="selectedChrmanagement.cardid" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="疾病名称" label-width="120px" prop="illname">
          <el-input v-model="selectedChrmanagement.illname" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="起始时间" label-width="120px" prop="starttime">
          <el-input v-model="selectedChrmanagement.starttime" autocomplete="off" type="date"></el-input>
        </el-form-item>
        <el-form-item label="终止时间" label-width="120px" prop="endtime">
          <el-input v-model="selectedChrmanagement.endtime" autocomplete="off" type="date"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onSubmit(selectedChrmanagement)">确 定</el-button>
      </div>
    </el-dialog>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
         <el-breadcrumb-item>信息管理</el-breadcrumb-item>
        <el-breadcrumb-item>报修信息</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <searchx @onSearch="searchResult" ref="Searchx"></searchx>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="chrmanagements"
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
          prop="ruralcard"
          label="农合证号"
          fit>
        </el-table-column>
        <el-table-column
          prop="chronid"
          label="慢病证号"
          fit>
        </el-table-column>
        <el-table-column
          prop="cardid"
          label="身份证号"
          fit>
        </el-table-column>
        <el-table-column
          prop="illname"
          label="疾病名称"
          fit>
        </el-table-column>
        <el-table-column
          prop="starttime"
          label="起始时间"
          fit>
        </el-table-column>
         <el-table-column
          prop="endtime"
          label="终止时间"
          show-overflow-tooltip
          fit>
        </el-table-column>
        <el-table-column
          label="操作"
          width="120">
          <template slot-scope="scope">
            <addBs @onSubmit="listBss()"></addBs>
            <el-button
              @click="editChrmanagement(scope.row)"
              type="text"
              size="small">
              编辑
            </el-button>
            <el-button
              @click.native.prevent="deleteChrmanagement(scope.row.id)"
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
    import Searchx from './Searchx'
    import addBs from './addBs'
    export default {
      name: 'Bsmanagement',
      components: {Searchx, addBs},
      data () {
          return {
            chrmanagements: [],
            dialogFormVisible: false,
            selectedChrmanagement: []
          }
      },
      mounted () {
        this.listChrmanagements()
      },
      computed: {
        tableHeight () {
          return window.innerHeight - 320
        }
      },
      methods: {
        listChrmanagements () {
          var _this = this
          this.$axios.get('/admin/chrmanagements/list').then(resp => {
            if (resp && resp.data.code === 200) {
              _this.chrmanagements = resp.data.result
            }
          })
        },
        searchResult () {
        var _this = this
        this.$axios
          .get('/searchx?cardid=' + this.$refs.Searchx.cardid, {
          }).then(resp => {
          if (resp && resp.data.code === 200) {
            _this.chrmanagements = resp.data.result
          } else {
              this.$alert(resp.data.message)
            }
        })
      },
        onSubmit (chrmanagement) {
          this.$axios.put('/admin/chrmanagement/editChrmanagement', {
            ruralcard: chrmanagement.ruralcard,
            chronid: chrmanagement.chronid,
            cardid: chrmanagement.cardid,
            illname: chrmanagement.illname,
            starttime: chrmanagement.starttime,
            endtime: chrmanagement.endtime
          }).then(resp => {
            if (resp && resp.data.code === 200) {
              this.$alert('用户信息修改成功')
              this.dialogFormVisible = false
              // 修改角色后重新请求用户信息，实现视图更新
              this.listChrmanagements()
            } else {
              this.$alert(resp.data.message)
            }
          })
        },
        editChrmanagement (chrmanagement) {
          this.dialogFormVisible = true
          this.selectedChrmanagement = chrmanagement
        },
      deleteChrmanagement (id) {
        this.$confirm('此操作将永久删除, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$axios
              .delete('/admin/chrmanagement/deleteChrmanagement/' + id).then(resp => {
              if (resp && resp.data.code === 200) {
                this.listChrmanagements()
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
