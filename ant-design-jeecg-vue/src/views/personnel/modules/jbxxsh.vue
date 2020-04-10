<template>
  <a-modal
    :title="title"
    :width="800"
    :visible="visible"
    :confirmLoading="confirmLoading"
    :destroyOnClose="destroyOnClose"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭"
  >
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">
        <a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol" label="审核结果">
          <a-select style="width: 100%" v-model="shjg">
            <a-select-option
              v-for="(item,index) in valueList"
              :key="index"
              :value="item.value"
            >{{item.name}}</a-select-option>
          </a-select>
        </a-form-item>
        <a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol" label="审核意见">
          <a-textarea v-model="shyj" :autoSize="true" />
        </a-form-item>
      </a-form>
    </a-spin>
  </a-modal>
</template>

<script>
import { postAction, putAction, getAction } from '@/api/manage'
import { httpAction } from '@/api/manage'
import pick from 'lodash.pick'
import moment from 'moment'

export default {
  name: 'FjgzxxModal',
  data() {
    return {
      title: '操作',
      visible: false,
      destroyOnClose: true,
      model: {},
      labelCol: {
        xs: { span: 24 },
        sm: { span: 5 }
      },
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 16 }
      },

      confirmLoading: false,
      form: this.$form.createForm(this),
      validatorRules: {},
      shjg: '',
      shyj: '',
      valueList: [],
      leibie: '',
      shujvid: ''
    }
  },
  props: ['sh', 'bs'],
  created() {},
  methods: {
    edit(e, record) {
        console.log(this.sh)
      this.shjg = ''
      this.shyj = ''
      this.leibie = e
      if (this.sh == '派出所审核') {
        this.valueList = [
          {
            value: '总局审核',
            name: '审核通过'
          },
          {
            value: '待审核',
            name: '不通过'
          }
        ]
      } else {
        this.valueList = [
          {
            value: '审核完成',
            name: '审核通过'
          },
          {
            value: '派出所审核',
            name: '不通过'
          }
        ]
      }
      if (e == '单') {
        this.shujvid = record.id
      } else {
        this.shujvid = ''
      }
      this.visible = true
    },
    close() {
      this.$emit('Refresh')
      this.visible = false
    },
    handleOk() {
      const that = this
      if (this.leibie == '单') {
        let obj = {
          id: this.shujvid,
          shjg: this.shjg,
          shyj: this.shyj,
          spbs: this.bs
        }
        that.confirmLoading = true
        getAction('/rsda/fjJbxx/dlsp', obj)
          .then(res => {
            if (res.success == true) {
              this.$message.success(res.message)
              that.close()
              this.$emit('close')
            } else {
              this.$message.error(res.message)
            }
          })
          .finally(() => {
            this.confirmLoading = false
          })
      } else {
        if (this.sh == '派出所审核') {
          this.pcsyjsh() //派出所一键审核
        } else {
          this.zjyjsh() //总局一键审核
        }
      }
    },
    //派出所一键审核
    pcsyjsh() {
      let obj = {
        shjg: this.shjg,
        shyj: this.shyj
      }
      this.confirmLoading = true
      getAction('/rsda/fjJbxx/pcsYjsp', obj)
        .then(res => {
          if (res.success == true) {
            this.$message.success(res.message)
            this.$emit('Refresh')
            this.close()
          } else {
            this.$message.error(res.message)
          }
        })
        .finally(() => {
          this.confirmLoading = false
        })
    },
    //总局一键审核
    zjyjsh() {
      let obj = {
        shjg: this.shjg,
        shyj: this.shyj
      }
      this.confirmLoading = true
      getAction('/rsda/fjJbxx/zjYjsp', obj)
        .then(res => {
          if (res.success == true) {
            this.$message.success(res.message)
            this.$emit('Refresh')
            this.close()
          } else {
            this.$message.error(res.message)
          }
        })
        .finally(() => {
          this.confirmLoading = false
        })
    },
    handleCancel() {
      this.close()
    }
  }
}
</script>

<style lang="less" scoped>
</style>