<template>
  <div style="text-align: left">
    <el-button class="add-button" type="success" @click="dialogFormVisible = true">增加家庭档案</el-button>
    <el-dialog
      title="增加家庭档案"
      :visible.sync="dialogFormVisible"
      @close="clear"
      width="25%">
      <el-form :model="familyForm" :rules="rules" label-position="left"
               label-width="0px">
        <el-form-item>
        <el-select v-model="familyForm.village" placeholder="所在村">
          <el-option label="林水村" value="4504210102"></el-option>
          <el-option label="恩义村" value="4504210101"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="familyForm.group" placeholder="所在组">
          <el-option label="林一组" value="450421010201"></el-option>
          <el-option label="林二组" value="450421010202"></el-option>
          <el-option label="林三组" value="450421010203"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="familyForm.housePro" placeholder="户属性">
          <el-option label="一般户" value="1"></el-option>
          <el-option label="五保户" value="2"></el-option>
          <el-option label="军烈属" value="3"></el-option>
          <el-option label="其他" value="9"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="familyForm.persname"
                    auto-complete="off" placeholder="户主姓名"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="familyForm.cardid"
                    auto-complete="off" placeholder="身份证号"></el-input>
        </el-form-item>
        <el-form-item>
           出生年月:<el-input type="date" v-model="familyForm.birthday"
                    auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item>
        <el-select v-model="familyForm.sex" placeholder="性别">
          <el-option label="男" value="1"></el-option>
          <el-option label="女" value="2"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="familyForm.perspro" placeholder="人员属性">
          <el-option label="一般户" value="1"></el-option>
          <el-option label="民政救助对象" value="2"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="familyForm.isrural" placeholder="是否农村户口">
          <el-option label="是" value="1"></el-option>
          <el-option label="否" value="0"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="familyForm.age"
                    auto-complete="off" placeholder="年龄"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="familyForm.liveAddress"
                    auto-complete="off" placeholder="家庭住址"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="familyForm.telephone"
                    auto-complete="off" placeholder="联系电话"></el-input>
        </el-form-item>
        <el-form-item style="width: 100%">
          <el-button type="primary" style="width: 40%;background: #505458;border: none" v-on:click="addFamily">添加</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: 'addFamily',
      data () {
        return {
          dialogFormVisible: false,
          familyForm: {
            village: '',
            group: '',
            housepro: '',
            persname: '',
            cardid: '',
            birthday: '',
            perspro: '',
            isrural: '',
            sex: '',
            age: '',
            liveaddress: '',
            telephone: ''

          }
        }
      },
      methods: {
        clear () {
          this.familyForm = {
            village: '',
            group: '',
            housepro: '',
            persname: '',
            cardid: '',
            birthday: '',
            perspro: '',
            isrural: '',
            sex: '',
            age: '',
            liveaddress: '',
            telephone: ''
          }
        },
        addFamily () {
          this.$axios.post('/admin/family/addFamily', {
              village: this.familyForm.village,
              group: this.familyForm.group,
              housepro: this.familyForm.housepro,
              persname: this.familyForm.persname,
              cardid: this.familyForm.cardid,
              birthday: this.familyForm.birthday,
              perspro: this.familyForm.perspro,
              isrural: this.familyForm.isrural,
              sex: this.familyForm.sex,
              age: this.familyForm.age,
              liveaddress: this.familyForm.liveaddress,
              telephone: this.familyForm.telephone
            })
             this.$axios.post('/admin/person/addPerson', {
              village: this.familyForm.village,
              group: this.familyForm.group,
              housepro: this.familyForm.housepro,
              persname: this.familyForm.persname,
              cardid: this.familyForm.cardid,
              birthday: this.familyForm.birthday,
              perspro: this.familyForm.perspro,
              isrural: this.familyForm.isrural,
              sex: this.familyForm.sex,
              age: this.familyForm.age,
              liveaddress: this.familyForm.liveaddress,
              telephone: this.familyForm.telephone
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
