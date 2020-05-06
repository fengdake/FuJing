<template>
  <a-modal
    :title="title"
    :width="1500"
    :visible="visible"
    :confirmLoading="confirmLoading"
    :destroyOnClose="destroyOnClose"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭"
  >
    <a-spin :spinning="confirmLoading">
      <div class="name">
        <span class>变更人姓名：</span>
        <span style="color: #0177d5;">2213213</span>
      </div>
      <div>
        <a-table
        ref="table"
        size="small"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        @change="handleTableChange"
        :scroll="{ x: 3500}"
      >
    
      </a-table>
      </div>
    </a-spin>
  </a-modal>
</template>

<script>
import { JeecgListMixin } from '@/mixins/chaxu'
import JEllipsis from '@/components/jeecg/JEllipsis'
import { postAction, putAction, getAction } from '@/api/manage'
import { httpAction } from '@/api/manage'
import pick from 'lodash.pick'
import moment from 'moment'

export default {
  name: 'FjgzxxModal',
  mixins: [JeecgListMixin, JEllipsis],
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
      columns: [
        {
          title: '序号',
          dataIndex: '',
          key: 'rowIndex',
          width: 60,
          align: 'center',
          customRender: function(t, r, index) {
            return parseInt(index) + 1
          },
          fixed: 'left'
        },
        {
          title: '姓名',
          align: 'center',
          dataIndex: 'xm',
          width: 100,
          fixed: 'left'
        },
        {
          title: '身份证号',
          align: 'center',
          dataIndex: 'sfzh',
          width: 150,
          fixed: 'left'
        },
        {
          title: '工资年月',
          align: 'center',
          dataIndex: 'gzny',
          width: 100,
          fixed: 'left'
        },
        {
          title: '所属单位',
          align: 'center',
          dataIndex: 'ssdw'
        },
        {
          title: '级别',
          align: 'center',
          dataIndex: 'jb',
          width: 100
        },
        {
          title: '辅警编号',
          align: 'center',
          dataIndex: 'fjbh',
          width: 100
        },
        {
          title: '减少时间',
          align: 'center',
          dataIndex: 'jsrq'
        },
        {
          title: '银行卡号',
          align: 'center',
          width: 200,
          dataIndex: 'yhkh'
        },
        {
          title: '工资发放银行',
          align: 'center',
          dataIndex: 'gzffyh'
        },
        {
          title: '保险基数',
          align: 'center',
          dataIndex: 'bxjs'
        },
        {
          title: '保险缴纳情况',
          align: 'center',
          dataIndex: 'bxjnqk',
          width: 200
        },
        {
          title: '基本工资',
          align: 'center',
          dataIndex: 'jbgz'
        },
        {
          title: '加款项:考核绩效（元）',
          align: 'center',
          dataIndex: 'jkxKhjx'
        },
        {
          title: '加款项:执勤补助（元）',
          align: 'center',
          dataIndex: 'jkxZqbz'
        },
        {
          title: '加款项:其他（元）',
          align: 'center',
          dataIndex: 'jkxQt'
        },
        {
          title: '加款项:考核扣款（元）',
          align: 'center',
          dataIndex: 'kkxKhkk'
        },
        {
          title: '扣款项:个人保险（元）',
          align: 'center',
          dataIndex: 'kkxGrbx'
        },
        {
          title: '扣款项:其他（元）',
          align: 'center',
          dataIndex: 'kkxQt'
        },
        {
          title: '实发数（元）',
          align: 'center',
          dataIndex: 'sfs'
        },
        {
          title: '审批人',
          align: 'center',
          dataIndex: 'gzxxSpr'
        },
        {
          title: '审批结果',
          align: 'center',
          dataIndex: 'spjg'
        },
        {
          title: '操作',
          dataIndex: 'action',
          align: 'center',
          scopedSlots: { customRender: 'action' },
          width: 100,
          fixed: 'right'
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
      this.visible = true
    //   this.getShujv(record.id) //获取修改数据
    },
    //获取修改数据
    getShujv(id) {
      let that = this
      let obj = {
        id: id
      }
      that.confirmLoading = true
      getAction('/rsda/fjJbxx/getSpsj', obj)
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
      this.$emit('Refresh')
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
      getAction('/rsda/fjJbxx/dlsp', obj)
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