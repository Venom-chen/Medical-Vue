<template>
  <div style="text-align: left">
    <el-button class="add-button" type="success" @click="dialogFormVisible = true">添加行政区域</el-button>
    <el-dialog
      title="添加行政区域"
      :visible.sync="dialogFormVisible"
      @close="clear"
      width="25%">
      <el-form :model="areaForm" :rules="rules" label-position="left"
               label-width="0px">

        <el-form-item >
          <el-input type="text" v-model="areaForm.areacode"
                    auto-complete="off" placeholder="行政区域编号"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="areaForm.areaname"
                    auto-complete="off" placeholder="行政区域名称"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="areaForm.grade"
                    auto-complete="off" placeholder="级别"></el-input>
        </el-form-item>
        <el-form-item style="width: 100%">
          <el-button type="primary" style="width: 40%;background: #505458;border: none" v-on:click="addArea">添加</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: 'addArea',
      data () {
        return {
          dialogFormVisible: false,
          areaForm: {
            areacode: '',
            areaname: '',
            grade: ''
          }
        }
      },
      methods: {
        clear () {
          this.areaForm = {
            areacode: '',
            areaname: '',
            grade: ''
          }
        },
        addArea () {
          this.$axios
            .post('/admin/area/addArea', {
              areacode: this.areaForm.areacode,
              areaname: this.areaForm.areaname,
              grade: this.areaForm.grade
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
