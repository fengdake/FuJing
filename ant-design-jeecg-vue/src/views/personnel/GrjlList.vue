<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
    </div>

    <!-- table区域-begin -->
    <div>
      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        @change="handleTableChange"
      >
        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" />
          <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
            <a>删除</a>
          </a-popconfirm>
        </span>
      </a-table>
    </div>
    <!-- table区域-end -->

    <!-- 表单区域 -->
    <grjl-modal ref="modalForm" @ok="modalFormOk" :sfzh="sfzh"></grjl-modal>
  </a-card>
</template>

<script>
import { httpAction, postAction, getAction } from '@/api/manage'
import GrjlModal from './modules/GrjlModal'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'

export default {
  name: 'GrjlList',
  mixins: [JeecgListMixin],
  components: {
    GrjlModal
  },
  data() {
    return {
      description: '人事档案-个人简历管理页面',
      // 表头
      columns: [
        {
          title: '序号',
          dataIndex: '',
          key: 'rowIndex',
          width: 60,
          align: 'center',
          customRender: function(t, r, index) {
            return parseInt(index) + 1
          }
        },
        {
          title: '开始日期',
          align: 'center',
          dataIndex: 'ksrq'
        },
        {
          title: '结束日期',
          align: 'center',
          dataIndex: 'jsrq'
        },
        {
          title: '具体内容',
          align: 'center',
          dataIndex: 'jtnr'
        },
        {
          title: '操作',
          dataIndex: 'action',
          align: 'center',
          scopedSlots: { customRender: 'action' }
        }
      ],
      url: {
        list: '/rsda/grjl/list',
        delete: '/rsda/grjl/delete',
        deleteBatch: '/rsda/grjl/deleteBatch',
        exportXlsUrl: 'rsda/grjl/exportXls',
        importExcelUrl: 'rsda/grjl/importExcel'
      }
    }
  },
  props: ['gerenid', 'sfzh'],
  computed: {
    importExcelUrl: function() {
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`
    }
  },
  methods: {
    sing() {
      this.loadData()
    },
    loadData() {
      let obj = {
        sfzh: this.sfzh
      }
      this.loading = true
      getAction('/rsda/grjl/list', obj)
        .then(res => {
          if (res.success == true) {
            this.dataSource = res.result.records
            this.ipagination.total = res.result.total
          }
        })
        .finally(() => {
          this.loading = false
        })
    },
    //新增
    handleAdd() {
      if (this.gerenid == '' || this.gerenid == null) {
        this.$message.warning('请先填写个人信息')
      } else {
        this.$refs.modalForm.add()
        this.$refs.modalForm.title = '新增'
      }
    }
  }
}
</script>
<style scoped>
@import '~@assets/less/common.less';
</style>