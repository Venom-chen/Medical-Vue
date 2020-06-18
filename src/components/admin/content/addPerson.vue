<template>
  <div style="text-align: left">
    <el-button class="add-button" type="success" @click="dialogFormVisible = true">添加成员</el-button>
    <el-dialog
      title="添加成员"
      :visible.sync="dialogFormVisible"
      @close="clear"
      width="25%">
      <el-form :model="personForm" :rules="rules" label-position="left"
               label-width="0px">
          <el-form-item>
          <el-input type="text" v-model="personForm.famicode"
                    auto-complete="off" placeholder="家庭编号"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="personForm.persname"
                    auto-complete="off" placeholder="成员姓名"></el-input>
        </el-form-item>
        <el-form-item>
        <el-select v-model="personForm.relation" placeholder="与户主关系">
          <el-option label="配偶" value="2"></el-option>
          <el-option label="子女" value="3"></el-option>
          <el-option label="父母" value="4"></el-option>
          <el-option label="外甥女" value="5"></el-option>
        </el-select>
        </el-form-item>
                <el-form-item>
          <el-input type="text" v-model="personForm.cardid"
                    auto-complete="off" placeholder="身份证号"></el-input>
        </el-form-item>
          <el-form-item>
           出生年月:<el-input type="date" v-model="personForm.birthday"
                    auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item>
        <el-select v-model="personForm.sex" placeholder="性别">
          <el-option label="男" value="1"></el-option>
          <el-option label="女" value="2"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="personForm.perspro" placeholder="人员属性">
          <el-option label="一般户" value="1"></el-option>
          <el-option label="民政救助对象" value="2"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="personForm.isrural" placeholder="是否农村户口">
          <el-option label="是" value="1"></el-option>
          <el-option label="否" value="0"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="personForm.age"
                    auto-complete="off" placeholder="年龄"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="personForm.liveAddress"
                    auto-complete="off" placeholder="家庭住址"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="personForm.telephone"
                    auto-complete="off" placeholder="联系电话"></el-input>
        </el-form-item>
        <el-form-item style="width: 100%">
          <el-button type="primary" style="width: 40%;background: #505458;border: none" v-on:click="addPerson">添加</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: 'addPerson',
      data () {
        return {
          dialogFormVisible: false,
          personForm: {
            famicode: '',
            relation: '',
            persname: '',
            cardid: '',
            birthday: '',
            perspro: '',
            isrural: '',
            age: '',
            liveaddress: '',
            telephone: '',
            sex: ''
          }
        }
      },
      methods: {
        clear () {
          this.personForm = {
            famicode: '',
            relation: '',
            persname: '',
            cardid: '',
            birthday: '',
            perspro: '',
            isrural: '',
            age: '',
            liveaddress: '',
            telephone: '',
            sex: ''
          }
        },
        addPerson () {
          this.$axios.post('/admin/persons/addPerson', {
              famicode: this.personForm.famicode,
              relation: this.personForm.relation,
              persname: this.personForm.persname,
              cardid: this.personForm.cardid,
              birthday: this.personForm.birthday,
              perspro: this.personForm.perspro,
              isrural: this.personForm.isrural,
              age: this.personForm.age,
              liveaddress: this.personForm.liveaddress,
              telephone: this.personForm.telephone,
              sex: this.personForm.sex
            })
            .then(resp => {
              if (resp.data.code === 200) {
                this.$alert('添加成功', '提示', {
                  confirmButtonText: '确定'
                })
                this.clear()
                this.$emit('onSubmit')
              } else {
                this.$alert(resp.data.message, '提示', {
                  confirmButtonText: '确定'
                })
              }
            })
            .catch(failResponse => {})
        }
      }
    }
</script>

<style scoped>
  .add-button {
    margin: 18px 0 0 10px;
  }
</style>
