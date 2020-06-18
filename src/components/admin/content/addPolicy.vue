<template>
  <div style="text-align: left">
    <el-button class="add-button" type="success" @click="dialogFormVisible = true">增加封顶线</el-button>
    <el-dialog
      title="增加封顶线"
      :visible.sync="dialogFormVisible"
      @close="clear"
      width="25%">
      <el-form :model="policyForm" :rules="rules" label-position="left"
               label-width="0px">
        <el-form-item >
          <el-input type="text" v-model="policyForm.runyear"
                    auto-complete="off" placeholder="执行年度"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="policyForm.maxline"
                    auto-complete="off" placeholder="封顶线"></el-input>
        </el-form-item>
        <el-form-item>
        <el-select v-model="policyForm.status" placeholder="状态">
          <el-option label="启用" value="1"></el-option>
          <el-option label="禁用" value="0"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="policyForm.remark"
                    auto-complete="off" placeholder="备注"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="policyForm.rate"
                    auto-complete="off" placeholder="报销比例"></el-input>
        </el-form-item>
        <el-form-item style="width: 100%">
          <el-button type="primary" style="width: 40%;background: #505458;border: none" v-on:click="addPolicy">添加</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: 'addPolicy',
      data () {
        return {
          dialogFormVisible: false,
          policyForm: {
            runyear: '',
            maxline: '',
            status: '',
            remark: '',
            rate: ''
          }
        }
      },
      methods: {
        clear () {
          this.policyForm = {
            runyear: '',
            maxline: '',
            status: '',
            remark: '',
            rate: ''
          }
        },
        addPolicy () {
          this.$axios
            .post('/admin/policy/addPolicy', {
              runyear: this.policyForm.runyear,
              maxline: this.policyForm.maxline,
              status: this.policyForm.status,
              remark: this.policyForm.remark,
              rate: this.policyForm.rate
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
