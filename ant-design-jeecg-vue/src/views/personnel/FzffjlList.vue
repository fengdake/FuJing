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
    <fzffjl-modal ref="modalForm" :sfzh="sfzh" @ok="modalFormOk"></fzffjl-modal>
  </a-card>
</template>

<script>
import { postAction, putAction, getAction } from '@/api/manage'
import FzffjlModal from './modules/FzffjlModal'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'

export default {
  name: 'FzffjlList',
  mixins: [JeecgListMixin],
  components: {
    FzffjlModal
  },
  data() {
    return {
      description: '服装发放记录管理页面',
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
          title: '发放时间',
          align: 'center',
          dataIndex: 'ffsj'
        },
        {
          title: '发放数量',
          align: 'center',
          dataIndex: 'ffsl'
        },
        {
          title: '具体明细',
          align: 'center',
          dataIndex: 'jtmx'
        },
        {
          title: '操作',
          dataIndex: 'action',
          align: 'center',
          scopedSlots: { customRender: 'action' }
        }
      ],
      url: {
        list: '/rsda/fzffjl/list',
        delete: '/rsda/fzffjl/delete',
        deleteBatch: '/rsda/fzffjl/deleteBatch',
        exportXlsUrl: 'rsda/fzffjl/exportXls',
        importExcelUrl: 'rsda/fzffjl/importExcel'
      }
    }
  },
  props: ['sfzh'],
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
      getAction('/rsda/fzffjl/list', obj)
        .then(res => {
          if (res.success == true) {
            this.dataSource = res.result.records
            this.ipagination.total = res.result.total
          }
        })
        .finally(() => {
          this.loading = false
        })
    }
  }
}
</script>
<style scoped>
@import '~@assets/less/common.less';
</style>