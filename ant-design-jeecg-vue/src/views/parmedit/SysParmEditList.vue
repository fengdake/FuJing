<template>
  <a-card :bordered="false">

    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="24">

          <a-col :md="6" :sm="8">
            <a-form-item label="所属类型">
              <a-input placeholder="请输入所属类型" v-model="queryParam.type"></a-input>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="类型分类">
              <a-input placeholder="请输入类型分类" v-model="queryParam.category"></a-input>
            </a-form-item>
          </a-col>
        <template v-if="toggleSearchStatus">
        <a-col :md="6" :sm="8">
            <a-form-item label="参数名">
              <a-input placeholder="请输入参数名" v-model="queryParam.name"></a-input>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="名称">
              <a-input placeholder="请输入名称" v-model="queryParam.titile"></a-input>
            </a-form-item>
          </a-col>
        </template>
          <a-col :md="6" :sm="8" >
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" ghost @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
              <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? '收起' : '展开' }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>

        </a-row>
      </a-form>
    </div>

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary" class="moBan" icon="download" @click="handleExportXls('可编辑表格配置')">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
        ref="table"
        size="small"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
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
    <sysParmEdit-modal ref="modalForm" @ok="modalFormOk"></sysParmEdit-modal>
  </a-card>
</template>

<script>
  import SysParmEditModal from './modules/SysParmEditModal'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'

  export default {
    name: "SysParmEditList",
    mixins:[JeecgListMixin],
    components: {
      SysParmEditModal
    },
    data () {
      return {
        description: '可编辑表格配置管理页面',
        // 表头
        columns: [
		   {
            title: '所属类型',
            align:"center",
            width:100,
            dataIndex: 'type'
           },
		   {
            title: '类型分类',
            align:"center",
         width:100,
            dataIndex: 'category'
           },
          {
            title: '序号',
            align:"center",
            width:60,
            dataIndex: 'ordernum'
          },
		   {
            title: '参数名',
            align:"center",
         width:70,
            dataIndex: 'name'
           },
		   {
            title: '名称',
            align:"center",
         width:100,
         scopedSlots: { customRender: 'taskResult' },
            dataIndex: 'titile'
           },
          {
            title: '是否必填',
            align:"center",
            width:80,
            dataIndex: 'require'
          },
		   {
            title: '参数类型',
            align:"center",
         width:80,
            dataIndex: 'paramtype'
           },
		   {
            title: '默认值',
            align:"center",
         width:70,
            dataIndex: 'defaultvalue'
           },
		   {
            title: '选项类型',
            align:"center",
         width:100,
         scopedSlots: { customRender: 'taskResult' },
            dataIndex: 'optionlist'
           },
		   {
            title: '日期格式类型',
            align:"center",
         width:100,
         scopedSlots: { customRender: 'taskResult' },
            dataIndex: 'dateformat'
           },
          {
            title: '操作',
            dataIndex: 'action',
            width:100,
            align:"center",
            scopedSlots: { customRender: 'action' },
          }
        ],
		url: {
          list: "/parmedit/sysParmEdit/list",
          delete: "/parmedit/sysParmEdit/delete",
          deleteBatch: "/parmedit/sysParmEdit/deleteBatch",
          exportXlsUrl: "parmedit/sysParmEdit/exportXls",
          importExcelUrl: "parmedit/sysParmEdit/importExcel",
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
  @import '~@assets/less/common.less';
  .moBan {
  background: #17c295;
  border: 1px solid #17c295;
}
.moBan:hover {
  background: #3ad0a8;
  border: 1px solid #17c295;
}
</style>