<template>
  <div style="text-align: left">
    <el-button class="add-button" type="success" @click="dialogFormVisible = true">增加慢性病信息</el-button>
    <el-dialog
      title="增加慢性病信息"
      :visible.sync="dialogFormVisible"
      @close="clear"
      width="25%">
      <el-form :model="ChronicdisForm" :rules="rules" label-position="left"
               label-width="0px">
        <el-form-item >
          <el-input type="text" v-model="ChronicdisForm.illcode"
                    auto-complete="off" placeholder="疾病编号"></el-input>
        </el-form-item>
        <el-form-item >
          <el-input type="text" v-model="ChronicdisForm.illname"
                    auto-complete="off" placeholder="疾病名称"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="ChronicdisForm.pycode"
                    auto-complete="off" placeholder="拼音码"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="ChronicdisForm.wbcode"
                    auto-complete="off" placeholder="五笔码"></el-input>
        </el-form-item>
        <el-form-item style="width: 100%">
          <el-button type="primary" style="width: 40%;background: #505458;border: none" v-on:click="addChronicdis">添加</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: 'addChronicdis',
      data () {
        return {
          dialogFormVisible: false,
          ChronicdisForm: {
            illcode: '',
            illname: '',
            pycode: '',
            wbcode: ''
          }
        }
      },
      methods: {
        clear () {
          this.ChronicdisForm = {
            illcode: '',
            illname: '',
            pycode: '',
            wbcode: ''
          }
        },
        addChronicdis () {
          this.$axios
            .post('/admin/chronicdis/addChronicdis', {
              illcode: this.ChronicdisForm.illcode,
              illname: this.ChronicdisForm.illname,
              pycode: this.ChronicdisForm.pycode,
              wbcode: this.ChronicdisForm.wbcode
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
