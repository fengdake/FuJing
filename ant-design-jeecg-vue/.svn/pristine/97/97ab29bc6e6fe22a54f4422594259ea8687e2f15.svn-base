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
        size="small"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        @change="handleTableChange">

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" />
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a style="color:red;">删除</a>
                </a-popconfirm>
        </span>
        <template slot="taskResult" slot-scope="text" style="width: 100%">
          <font :title="text" style=" display: inline-block;width:100px;white-space: nowrap;text-overflow: ellipsis;overflow: hidden;line-height: 100%" >{{text}}</font>
        </template>
      </a-table>
    </div>
    <!-- table区域-end -->

    <!-- 表单区域 -->
    <sysVersion-modal ref="modalForm" @ok="modalFormOk"></sysVersion-modal>
  </a-card>
</template>

<script>
  import SysVersionModal from './modules/SysVersionModal'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'

  export default {
    name: "SysVersionList",
    mixins:[JeecgListMixin],
    components: {
      SysVersionModal
    },
    data () {
      return {
        description: '系统版本管理页面',
        // 表头
        columns: [
		   {
            title: '移动端系统版本',
            align:"center",
            dataIndex: 'mobileVersion'
           },
		   {
            title: '此次为测试更新',
            align:"center",
            dataIndex: 'versionName'
           },
		   {
            title: '更新内容',
            align:"center",
            dataIndex: 'updateContent'
           },
		   {
            title: 'app大小',
            align:"center",
            dataIndex: 'updateSize'
           },
		   {
            title: '是否强制更新',
            align:"center",
            dataIndex: 'isForceUpdate',
         customRender: function(text) {
           if (text == '0') {
             return '禁用'
           } else if (text == '1') {
             return '启用'
           }else {
             return text
           }
         }
       },
		   {
            title: '更新地址',
            align:"center",
            scopedSlots: { customRender: 'taskResult' },
            dataIndex: 'packageUrl'
           },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            scopedSlots: { customRender: 'action' },
          }
        ],
		url: {
          list: "/system/sysVersion/list",
          delete: "/system/sysVersion/delete",
          deleteBatch: "/system/sysVersion/deleteBatch",
          exportXlsUrl: "system/sysVersion/exportXls",
          importExcelUrl: "system/sysVersion/importExcel",
       },
    }
  },
  computed: {
    importExcelUrl: function(){
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
    }
  },
    methods: {
     
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less'
</style>