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
      <div style="display: flex; align-items: center;">
        <span class="xian"></span>
        <span class="title">变更信息内容</span>
      </div>
      <div class="name">
        <span class>变更人姓名：</span>
        <span style="color: #0177d5;">{{xm}}</span>
      </div>
      <div>
        <a-table
          ref="table"
          bordered
          rowKey="id"
          size="small"
          :pagination="false"
          :loading="loading"
          @change="handleTableChange"
          :columns="columns"
          :dataSource="dataSource"
        >
          <a slot="bghxx" slot-scope="text">{{text}}</a>
        </a-table>
      </div>
      <div style="display: flex; align-items: center;margin: 25px 0;">
        <span class="xian"></span>
        <span class="title">审核结果</span>
      </div>
      <a-form :form="form">
        <a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol" label="审批人">
          <a-input v-model="shenPiRen" disabled />
        </a-form-item>
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
import { JeecgListMixin } from '@/mixins/chaxu'
import { postAction, putAction, getAction } from '@/api/manage'
import { httpAction } from '@/api/manage'
import pick from 'lodash.pick'
import moment from 'moment'

export default {
  name: 'FjgzxxModal',
  mixins: [JeecgListMixin],
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
      // 表头
      columns: [
        {
          title: '序号',
          dataIndex: '',
          key: 'rowIndex',
          align: 'center',
          ssdwlist: [],
          customRender: function(t, r, index) {
            return parseInt(index) + 1
          }
        },
        {
          title: '项目',
          align: 'center',
          dataIndex: 'pro'
        },
        {
          title: '变更前信息',
          align: 'center',
          dataIndex: 'bgqxx'
        },
        {
          title: '变更后信息',
          align: 'center',
          dataIndex: 'bghxx',
          scopedSlots: { customRender: 'bghxx' }
        }
      ],
      confirmLoading: false,
      form: this.$form.createForm(this),
      validatorRules: {},
      xm: '',
      shjg: '',
      shyj: '',
      shenPiRen: '',
      valueList: [],
      leibie: '',
      shujvid: '',
      bs: 'bj'
    }
  },
  props: ['sh'],
  created() {},
  methods: {
    edit(record) {
      this.xm = ''
      this.shenPiRen = ''
      this.shjg = ''
      this.shyj = ''
      this.shujvid = record.fzxxId
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
      this.visible = true
      this.getShujv(record.fzxxId) //获取修改数据
    },
    //获取修改数据
    getShujv(id) {
      let that = this
      let obj = {
        id: id
      }
      that.confirmLoading = true
      getAction('/rsda/fzxx/getSpsj', obj)
        .then(res => {
          this.xm = res.bgrxm
          this.shenPiRen = res.spr
          this.dataSource = res.bglb
        })
        .finally(() => {
          this.confirmLoading = false
        })
    },
    close() {
      this.$emit('modalFormOk')
      this.visible = false
    },
    handleOk() {
      const that = this
      let obj = {
        id: this.shujvid,
        shjg: this.shjg,
        shyj: this.shyj,
        spbs: this.bs
      }
      that.confirmLoading = true
      getAction('/rsda/fzxx/dlsp', obj)
        .then(res => {
          if (res.success == true) {
            this.$message.success(res.message)
            that.close()
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
.xian {
  display: inline-block;
  width: 3px;
  height: 16px;
  background-color: #0177d5;
  margin-right: 8px;
}
.title {
  font-size: 16px;
  color: #0177d5;
}
.name {
  text-align: center;
  font-size: 18px;
  margin-bottom: 18px;
}
</style>