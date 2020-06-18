<template>
  <div style="text-align: left">
    <el-button class="add-button" type="success" @click="dialogFormVisible = true">增加医疗机构信息</el-button>
    <el-dialog
      title="增加医疗机构信息"
      :visible.sync="dialogFormVisible"
      @close="clear"
      width="25%">
      <el-form :model="medicalForm" :rules="rules" label-position="left"
               label-width="0px">
        <el-form-item >
          <el-input type="text" v-model="medicalForm.jgbm"
                    auto-complete="off" placeholder="医疗机构编号"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="medicalForm.zzjgbm"
                    auto-complete="off" placeholder="组织机构级别"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="medicalForm.jgmc"
                    auto-complete="off" placeholder="机构名称"></el-input>
        </el-form-item>
        <el-form-item>
        <el-select v-model="medicalForm.dqbm" placeholder="地区编码">
          <el-option label="苍梧县新农合管理中心" value="450421"></el-option>
          <el-option label="龙圩镇合管办" value="45042101"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="medicalForm.areacode" placeholder="行政区域编码">
          <el-option label="苍梧县" value="450421"></el-option>
          <el-option label="龙圩镇" value="45042101"></el-option>
          <el-option label="恩义村" value="4504210101"></el-option>
          <el-option label="多一组" value="450421010101"></el-option>
          <el-option label="多二组" value="450421010102"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="medicalForm.lsgx" placeholder="隶属关系">
          <el-option label="县" value="50"></el-option>
          <el-option label="镇" value="62"></el-option>
          <el-option label="乡" value="63"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="medicalForm.jgjb" placeholder="机构级别">
          <el-option label="村卫生室" value="1"></el-option>
          <el-option label="乡镇卫生院" value="2"></el-option>
          <el-option label="县级医疗机构" value="3"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="medicalForm.sbddlx" placeholder="申请定点类型">
          <el-option label="综合定点" value="1"></el-option>
          <el-option label="门诊定点" value="2"></el-option>
          <el-option label="住院定点" value="3"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="medicalForm.pzddlx" placeholder="批准定点类型">
          <el-option label="综合定点" value="1"></el-option>
          <el-option label="门诊定点" value="2"></el-option>
          <el-option label="住院定点" value="3"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="medicalForm.ssjjlx" placeholder="所属经济类型">
          <el-option label="内资" value="10"></el-option>
          <el-option label="国有全资" value="11"></el-option>
          <el-option label="集体全资" value="12"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item>
        <el-select v-model="medicalForm.wsjgdl" placeholder="卫生机构大类">
          <el-option label="A100" value="10"></el-option>
          <el-option label="C220" value="11"></el-option>
          <el-option label="C210" value="12"></el-option>
        </el-select>
        </el-form-item>
         <el-form-item>
          <el-input type="text" v-model="medicalForm.wsjgxl"
                    auto-complete="off" placeholder="卫生机构小类"></el-input>
        </el-form-item>
         <el-form-item>
          <el-input type="text" v-model="medicalForm.zgdw"
                    auto-complete="off" placeholder="主管单位"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="date" v-model="medicalForm.kysj"
                    auto-complete="off" placeholder="开业时间"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="medicalForm.frdb"
                    auto-complete="off" placeholder="法人代表"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" v-model="medicalForm.zczj"
                    auto-complete="off" placeholder="注册资金"></el-input>
        </el-form-item>
        <el-form-item style="width: 100%">
          <el-button type="primary" style="width: 40%;background: #505458;border: none" v-on:click="addMedical">添加</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: 'addMedical',
      data () {
        return {
          dialogFormVisible: false,
          medicalForm: {
            jgbm: '',
            zzjgbm: '',
            jgmc: '',
            dqbm: '',
            areacode: '',
            lsgx: '',
            jgjb: '',
            sbddlx: '',
            pzddlx: '',
            ssjjlx: '',
            wsjgdl: '',
            wsjgxl: '',
            zgdw: '',
            kysj: '',
            frdb: '',
            zczj: ''
          }
        }
      },
      methods: {
        clear () {
          this.medicalForm = {
            jgbm: '',
            zzjgbm: '',
            jgmc: '',
            dqbm: '',
            areacode: '',
            lsgx: '',
            jgjb: '',
            sbddlx: '',
            pzddlx: '',
            ssjjlx: '',
            wsjgdl: '',
            wsjgxl: '',
            zgdw: '',
            kysj: '',
            frdb: '',
            zczj: ''
          }
        },
        addMedical () {
          this.$axios
            .post('/admin/medical/addMedical', {
              jgbm: this.medicalForm.jgbm,
              zzjgbm: this.medicalForm.zzjgbm,
              jgmc: this.medicalForm.jgmc,
              dqbm: this.medicalForm.dqbm,
              areacode: this.medicalForm.areacode,
              lsgx: this.medicalForm.lsgx,
              jgjb: this.medicalForm.jgjb,
              sbddlx: this.medicalForm.sbddlx,
              pzddlx: this.medicalForm.pzddlx,
              ssjjlx: this.medicalForm.ssjjlx,
              wsjgdl: this.medicalForm.wsjgdl,
              wsjgxl: this.medicalForm.wsjgxl,
              zgdw: this.medicalForm.zgdw,
              kysj: this.medicalForm.kysj,
              frdb: this.medicalForm.frdb,
              zczj: this.medicalForm.zczj
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
