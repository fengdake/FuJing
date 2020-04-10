<template>
  <a-card :bordered="false">
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
    <jcxx-modal ref="modalForm" @ok="modalFormOk" :sfzh="sfzh"></jcxx-modal>
  </a-card>
</template>

<script>
import { httpAction, postAction, getAction } from '@/api/manage'
import JcxxModal from './modules/JcxxModal'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'

export default {
  name: 'JcxxList',
  mixins: [JeecgListMixin],
  components: {
    JcxxModal
  },
  data() {
    return {
      description: '人事档案-奖惩信息管理页面',
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
          title: '时间',
          align: 'center',
          dataIndex: 'sj'
        },
        {
          title: '奖惩级别',
          align: 'center',
          dataIndex: 'jcjb'
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
        list: '/rsda/jcxx/list',
        delete: '/rsda/jcxx/delete',
        deleteBatch: '/rsda/jcxx/deleteBatch',
        exportXlsUrl: 'rsda/jcxx/exportXls',
        importExcelUrl: 'rsda/jcxx/importExcel'
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
    shuaxin() {
      this.loadData()
    },
    loadData() {
      let obj = {
        sfzh: this.sfzh
      }
      this.loading = true
      getAction('/rsda/jcxx/list', obj)
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