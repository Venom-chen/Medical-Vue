<template>
  <div style="text-align: left">
    <el-button class="add-button" type="success" @click="dialogFormVisible = true">增加慢性病证信息</el-button>
    <el-dialog
      title="增加慢性病证信息"
      :visible.sync="dialogFormVisible"
      @close="clear"
      width="25%">
      <el-form :model="ChrmanagementForm" :rules="rules" label-position="left"
               label-width="0px">
        <el-form-item >
          <el-input type="text" v-model="ChrmanagementForm.ruralcard"
                    auto-complete="off" placeholder="农合证号"></el-input>
        </el-form-item>
        <el-form-item >
          <el-input type="text" v-model="ChrmanagementForm.chronid"
                    auto-complete="off" placeholder="慢病证号"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="ChrmanagementForm.cardid"
                    auto-complete="off" placeholder="身份证号"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="ChrmanagementForm.illname"
                    auto-complete="off" placeholder="疾病名称"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="date" v-model="ChrmanagementForm.starttime"
                    auto-complete="off" placeholder="起始时间" ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="date" v-model="ChrmanagementForm.endtime"
                    auto-complete="off" placeholder="终止时间"></el-input>
        </el-form-item>
        <el-form-item style="width: 100%">
          <el-button type="primary" style="width: 40%;background: #505458;border: none" v-on:click="addChrmanagement">添加</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: 'addChrmanagement',
      data () {
        return {
          dialogFormVisible: false,
          ChrmanagementForm: {
            ruralcard: '',
            chronid: '',
            cardid: '',
            illname: '',
            starttime: '',
            endtime: ''
          }
        }
      },
      methods: {
        clear () {
          this.ChrmanagementForm = {
            ruralcard: '',
            chronid: '',
            cardid: '',
            illname: '',
            starttime: '',
            endtime: ''
          }
        },
        addChrmanagement () {
          this.$axios
            .post('/admin/chrmanagement/addChrmanagement', {
              ruralcard: this.ChrmanagementForm.ruralcard,
              chronid: this.ChrmanagementForm.chronid,
              cardid: this.ChrmanagementForm.cardid,
              illname: this.ChrmanagementForm.illname,
              starttime: this.ChrmanagementForm.starttime,
              endtime: this.ChrmanagementForm.endtime
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
