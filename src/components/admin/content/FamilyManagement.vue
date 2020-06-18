<template>
  <div>
    <el-dialog
      title="修改家庭信息"
      :visible.sync="dialogFormVisible">
      <el-form v-model="selectedFamily" style="text-align: left" ref="dataForm">
        <el-form-item label="家庭编号" label-width="120px" prop="famicode">
          <label>{{selectedFamily.famicode}}</label>
        </el-form-item>
        <el-form-item label="所在村编号" label-width="120px" prop="village">
          <el-input v-model="selectedFamily.village" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="户主姓名" label-width="120px" prop="persname">
          <el-input v-model="selectedFamily.persname" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="家庭人口数" label-width="120px" prop="popunum">
          <el-input v-model="selectedFamily.popunum" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="农业人口数" label-width="120px" prop="agrinum">
          <el-input v-model="selectedFamily.agrinum" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="建档时间" label-width="120px" prop="creattime">
          <el-input v-model="selectedFamily.creattime" autocomplete="off" type="date"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onSubmit(selectedFamily)">确 定</el-button>
      </div>
    </el-dialog>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
         <el-breadcrumb-item>信息管理</el-breadcrumb-item>
        <el-breadcrumb-item>工作内容信息</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <addFamily @onSubmit="listFamilys()"></addFamily>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="familys"
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
          prop="famicode"
          label="家庭编号"
          fit>
        </el-table-column>
        <el-table-column
          prop="village"
          label="所在村编号"
          fit>
        </el-table-column>
        <el-table-column
          prop="persname"
          label="户主姓名"
          fit>
        </el-table-column>
        <el-table-column
          prop="popunum"
          label="家庭人口数"
          fit>
        </el-table-column>
        <el-table-column
          prop="agrinum"
          label="农业人口数"
          fit>
        </el-table-column>
        <el-table-column
          prop="creattime"
          label="建档时间"
          show-overflow-tooltip
          fit>
        </el-table-column>
        <el-table-column
          label="操作"
          width="120">
          <template slot-scope="scope">
            <addPerson @onSubmit="listFamilys()"></addPerson>
            <el-button
              @click="editFamily(scope.row)"
              type="text"
              size="small">
              编辑
            </el-button>
            <el-button
              @click.native.prevent="deleteFamily(scope.row.id)"
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
  import addFamily from './addFamily'
  import addPerson from './addPerson'
    export default {
      name: 'FamilyManagement',
      components: {addFamily, addPerson},
      data () {
          return {
            familys: [],
            dialogFormVisible: false,
            selectedFamily: []
          }
      },
      mounted () {
        this.listFamilys()
      },
      computed: {
        tableHeight () {
          return window.innerHeight - 320
        }
      },
      methods: {
        listFamilys () {
          var _this = this
          this.$axios.get('/admin/family/list').then(resp => {
            if (resp && resp.data.code === 200) {
              _this.familys = resp.data.result
            }
          })
        },
        onSubmit (family) {
          this.$axios.put('/admin/family/editFamily', {
            famicode: family.famicode,
            village: family.village,
            town: family.town,
            persname: family.persname,
            popunum: family.popunum,
            agrinum: family.agrinum,
            creattime: family.creattime
          }).then(resp => {
            if (resp && resp.data.code === 200) {
              this.$alert('用户信息修改成功')
              this.dialogFormVisible = false
              // 修改角色后重新请求用户信息，实现视图更新
              this.listFamilys()
            } else {
              this.$alert(resp.data.message)
            }
          })
        },
        editFamily (family) {
          this.dialogFormVisible = true
          this.selectedFamily = family
        },
      deleteFamily (id) {
        this.$confirm('此操作将永久删除, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$axios
              .delete('/admin/family/deleteFamily/' + id).then(resp => {
              if (resp && resp.data.code === 200) {
                this.listFamilys()
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
