<template>
  <a-card :bordered="false">

    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="24">

          <a-col :md="6" :sm="8">
            <a-form-item label="职位名称">
              <a-input placeholder="请输入职位名称" v-model="queryParam.positionName"></a-input>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="职位代码">
              <a-input placeholder="请输入职位代码" v-model="queryParam.positionCode"></a-input>
            </a-form-item>
          </a-col>
        <template v-if="toggleSearchStatus">
        <a-col :md="6" :sm="8">
            <a-form-item label="部门类型">
              <a-select v-model="queryParam.positionType" placeholder="请输入部门类型">
                <a-select-option value="1">总局</a-select-option>
                <a-select-option value="2">分局</a-select-option>
                <a-select-option value="3">其他</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="选用状态">
              <a-select v-model="queryParam.status" placeholder="请输入选用状态">
                <a-select-option value="0">否</a-select-option>
                <a-select-option value="1">是</a-select-option>
              </a-select>
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
      <a-button type="primary" class="moBan" icon="download" @click="handleExportXls('职位信息')">导出</a-button>
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
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>
    <!-- table区域-end -->

    <!-- 表单区域 -->
    <sysPosition-modal ref="modalForm" @ok="modalFormOk"></sysPosition-modal>

    <!-- 表单区域 -->
    <sysPos-modal ref="modalForm2" @ok="modalFormOk2"/>
  </a-card>
</template>

<script>
  import SysPositionModal from './modules/SysPositionModal'
  import SysPosModal from './modules/SysPosModal'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'

  export default {
    name: "SysPositionList",
    mixins:[JeecgListMixin],
    components: {
      SysPositionModal,SysPosModal
    },
    data () {
      return {
        description: '职位信息管理页面',
        // 表头
        columns: [
          {
            title: '序号',
            dataIndex: '',
            key:'rowIndex',
            width:60,
            align:"center",
            customRender:function (t,r,index) {
              return parseInt(index)+1;
            }
           },
		   {
            title: '职位名称',
            align:"center",
            dataIndex: 'positionName'
           },
		   {
            title: '排序',
            align:"center",
            dataIndex: 'positionOrder'
           },
		   {
            title: '部门类型',
            align:"center",
            dataIndex: 'positionType',
             customRender: function(text) {
               if (text == 1) {
                 return '总行'
               } else if (text == 2) {
                 return '支行'
               } else if (text == 3) {
                 return '机关部室'
               } else if (text == 4) {
                 return '其他'
               }else {
                 return text
               }
             }
           },
		   {
            title: '职位代码',
            align:"center",
            dataIndex: 'positionCode'
           },
		   {
            title: '选用状态',
            align:"center",
            dataIndex: 'status',
           customRender: function(text) {
               if (text == 0) {
                 return '否'
               } else if (text == 1) {
                 return '是'
               }else {
                 return text
               }
         }
           },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            scopedSlots: { customRender: 'action' },
          }
        ],
		url: {
          list: "/system/sysPosition/list",
          delete: "/system/sysPosition/delete",
          deleteBatch: "/system/sysPosition/deleteBatch",
          exportXlsUrl: "system/sysPosition/exportXls",
          importExcelUrl: "system/sysPosition/importExcel",
       },
    }
  },
  computed: {
    importExcelUrl: function(){
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
    }
  },
    methods: {
      handleEdit2: function (record) {
        this.$refs.modalForm2.edit(record);
        this.$refs.modalForm2.title = "查看";
      },
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