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
        <a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol" label="工资年月">
          <a-month-picker style="width:100%;" @change="onChange" placeholder="选择工资年月" />
        </a-form-item>
          <div style="padding-left: 98px;text-align: right;" v-html="tishi"></div>
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
      gzny: '',
      tishi: ''
    }
  },
  props: [],
  created() {},
  methods: {
    edit() {
      this.gzny = ''
      this.tishi = ''
      this.visible = true
    },
    close() {
      this.$emit('ok')
      this.visible = false
    },
    onChange(date, dateString) {
      console.log(date, dateString)
      this.gzny = dateString
      this.tijiao(dateString) //提交
    },
    //提交
    tijiao(dateString) {
      let obj = {
        gzny: dateString
      }
      this.confirmLoading = true
      getAction('/rsda/fjgzxx/getSumSfs', obj)
        .then(res => {
          console.log(res)
          if (JSON.stringify(res) == '{}') {
            this.tishi = '当前月份没有人需要提交审核'
          } else {
            this.tishi = '本次提交待审核人数<span style="color:blue;font-weight: 500;"> ' + res.dshrs + ' </span>人，' + '工资总额<span style="color:blue;font-weight: 500;"> ' + res.sfsNum+' </span>元'
          }
        })
        .finally(() => {
          this.confirmLoading = false
        })
    },
    handleOk() {
      let obj = {
        gzny: this.gzny
      }
      this.confirmLoading = true
      getAction('/rsda/fjgzxx/tjshHandler', obj)
        .then(res => {
          if (res.success == true) {
            this.$message.success(res.message)
            this.$emit('ok')
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
/deep/ .ant-modal-body {
    padding: 0px !important;
    font-size: 12px;
    line-height: 1.5;
    word-wrap: break-word;
}
</style>