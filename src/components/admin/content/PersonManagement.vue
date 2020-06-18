<template>
  <div>
    <el-dialog
      title="录入信息"
      :visible.sync="dialogFormVisible">
      <el-form v-model="selectedPerson" style="text-align: left" ref="dataForm">
         <el-form-item label="农合证号" label-width="120px" prop="perscode">
          <label>{{selectedPerson.perscode}}</label>
        </el-form-item>
        <el-form-item label="家庭编号" label-width="120px" prop="famicode">
          <label>{{selectedPerson.famicode}}</label>
        </el-form-item>
        <el-form-item label="个人姓名" label-width="120px" prop="persname">
          <el-input v-model="selectedPerson.persname" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="与户主关系" label-width="120px" prop="relation">
        <el-select v-model="selectedPerson.relation" autocomplete="off">
          <el-option label="配偶" value="2"></el-option>
          <el-option label="子女" value="3"></el-option>
          <el-option label="父母" value="4"></el-option>
          <el-option label="外甥女" value="5"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item label="性别" label-width="120px" prop="sex">
        <el-select v-model="selectedPerson.sex" autocomplete="off">
          <el-option label="男" value="1"></el-option>
          <el-option label="女" value="2"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item label="出生年月" label-width="120px" prop="birthday">
          <el-input v-model="selectedPerson.birthday" autocomplete="off" type="date"></el-input>
        </el-form-item>
        <el-form-item label="年龄" label-width="120px" prop="age">
          <el-input v-model="selectedPerson.age" autocomplete="off"></el-input>
        </el-form-item>
         <el-form-item label="身份证号" label-width="120px" prop="cardid">
          <el-input v-model="selectedPerson.cardid" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onSubmit(selectedPerson)">确 定</el-button>
      </div>
    </el-dialog>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
         <el-breadcrumb-item>信息管理</el-breadcrumb-item>
        <el-breadcrumb-item>档案信息</el-breadcrumb-item>
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
          prop="perscode"
          label="农合证号"
          fit>
        </el-table-column>
        <el-table-column
          prop="famicode"
          label="家庭编号"
          fit>
        </el-table-column>
        <el-table-column
          prop="persname"
          label="个人姓名"
          fit>
        </el-table-column>
         <el-table-column
          prop="relation"
          label="与户主关系"
          fit>
        </el-table-column>
         <el-table-column
          prop="sex"
          label="性别"
          fit>
        </el-table-column>
         <el-table-column
          prop="birthday"
          label="出生年月"
          fit>
        </el-table-column>
         <el-table-column
          prop="age"
          label="年龄"
          fit>
        </el-table-column>
         <el-table-column
          prop="cardid"
          label="身份证号"
          show-overflow-tooltip
          fit>
        </el-table-column>
        <el-table-column
          label="操作"
          width="120">
          <template slot-scope="scope">
            <el-button
              @click="editPerson(scope.row)"
              type="text"
              size="small">
              修改
            </el-button>
            <el-button
              @click.native.prevent="deletePerson(scope.row.id)"
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
    export default {
      name: 'PersonManagement',
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
        listPersons () {
          var _this = this
          this.$axios.get('/admin/person/list').then(resp => {
            if (resp && resp.data.code === 200) {
              _this.persons = resp.data.result
            }
          })
        },
        onSubmit (person) {
          this.$axios.put('/admin/person/editPerson', {
            perscode: person.perscode,
            famicode: person.famicode,
            persname: person.persname,
            relation: person.relation,
            sex: person.sex,
            birthday: person.birthday,
            age: person.age,
            cardid: person.cardid
          }).then(resp => {
            if (resp && resp.data.code === 200) {
              this.$alert('用户信息修改成功')
              this.dialogFormVisible = false
              // 修改角色后重新请求用户信息，实现视图更新
              this.listPersons()
            } else {
              this.$alert(resp.data.message)
            }
          })
        },
        editPerson (person) {
          this.dialogFormVisible = true
          this.selectedPerson = person
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
                this.listPersons()
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
