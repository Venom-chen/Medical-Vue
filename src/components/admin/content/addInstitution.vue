<template>
  <div style="text-align: left">
    <el-button class="add-button" type="success" @click="dialogFormVisible = true">填写农合机构信息</el-button>
    <el-dialog
      title="填写农合机构信息"
      :visible.sync="dialogFormVisible"
      @close="clear"
      width="25%">
      <el-form :model="insForm" :rules="rules" label-position="left"
               label-width="0px">
        <el-select v-model="insForm.areacode" placeholder="所属地区">
          <el-option label="苍梧县" value="450421"></el-option>
          <el-option label="龙圩镇" value="45042101"></el-option>
          <el-option label="恩义村" value="4504210101"></el-option>
          <el-option label="多一组" value="450421010101"></el-option>
          <el-option label="多二组" value="450421010102"></el-option>
          <el-option label="瓦窑组" value="450421010103"></el-option>
          <el-option label="猪腰组" value="450421010104"></el-option>
          <el-option label="头塘组" value="450421010105"></el-option>
          <el-option label="林水村" value="4504210102"></el-option>
          <el-option label="林一组" value="450421010201"></el-option>
          <el-option label="林二组" value="450421010202"></el-option>
          <el-option label="林三组" value="450421010203"></el-option>
        </el-select>
        <el-form-item>
          <el-input type="text" v-model="insForm.agencode"
                    auto-complete="off" placeholder="经办机构编码"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="insForm.agenname"
                    auto-complete="off" placeholder="经办机构编码名称"></el-input>
        </el-form-item>
        <el-form-item style="width: 100%">
          <el-button type="primary" style="width: 40%;background: #505458;border: none" v-on:click="addInstitution">添加</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: 'addInstitution',
      data () {
        return {
          dialogFormVisible: false,
          insForm: {
            agencode: '',
            agenname: '',
             areacode: ''
          }
        }
      },
      methods: {
        clear () {
          this.insForm = {
            areacode: '',
            agencode: '',
            agenname: ''
          }
        },
        addInstitution () {
          this.$axios
            .post('/admin/institution/addInstitution/', {
              areacode: this.insForm.areacode,
              agencode: this.insForm.agencode,
              agenname: this.insForm.agenname
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
