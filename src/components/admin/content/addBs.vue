<template>
  <div style="text-align: left">
    <el-button class="add-button" type="success" @click="dialogFormVisible = true">填写报销</el-button>
    <el-dialog
      title="填写报销"
      :visible.sync="dialogFormVisible"
      @close="clear"
      width="25%">
      <el-form :model="bsForm" :rules="rules" label-position="left"
               label-width="0px">

        <el-form-item >
          <el-input type="text" v-model="bsForm.illness"
                    auto-complete="off" placeholder="疾病名称"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="bsForm.expenses"
                    auto-complete="off" placeholder="总费用"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="bsForm.invoiceno"
                    auto-complete="off" placeholder="发票号"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="date" v-model="bsForm.clinicaltime"
                    auto-complete="off" placeholder="就诊时间"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="bsForm.cardid"
                    auto-complete="off" placeholder="身份证"></el-input>
        </el-form-item>
        <el-form-item style="width: 100%">
          <el-button type="primary" style="width: 40%;background: #505458;border: none" v-on:click="addBs">添加</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: 'addBs',
      data () {
        return {
          dialogFormVisible: false,
          bsForm: {
            illness: '',
            expenses: '',
            invoiceno: '',
            clinicaltime: '',
            cardid: ''
          }
        }
      },
      methods: {
        clear () {
          this.bsForm = {
            illness: '',
            expenses: '',
            invoiceno: '',
            clinicaltime: '',
            cardid: ''
          }
        },
        addBs () {
          this.$axios
            .post('/admin/bs/addBs', {
              illness: this.bsForm.illness,
              expenses: this.bsForm.expenses,
              invoiceno: this.bsForm.invoiceno,
              cardid: this.bsForm.cardid,
              clinicaltime: this.bsForm.clinicaltime
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
