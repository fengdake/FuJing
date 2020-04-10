<template>
  <a-card :bordered="false">

    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="24">

          <a-col :md="6" :sm="8">
            <a-form-item label="岗位名称">
              <a-input placeholder="请输入岗位名称" v-model="queryParam.jobName"></a-input>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="职位代码">
              <a-input placeholder="请输入职位代码" v-model="queryParam.jobCode"></a-input>
            </a-form-item>
          </a-col>
        <template v-if="toggleSearchStatus">
        <a-col :md="6" :sm="8">
            <a-form-item label="对应职位">
              <a-select
                style="width: 100%"
                placeholder="请选择对应职位" v-model="queryParam.jobPosition">
                <a-select-option  v-for="(item,index) in jobList" :key="index.toString()" :value="item.value">
                  {{ item.text }}
                </a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="理论工资">
              <a-input placeholder="请输入理论工资" v-model="queryParam.jobPay"></a-input>
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
      <a-button type="primary" class="moBan" icon="download" @click="handleExportXls('岗位信息')">导出</a-button>
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
    <sysJob-modal ref="modalForm" @ok="modalFormOk"></sysJob-modal>
  </a-card>
</template>

<script>
  import SysJobModal from './modules/SysJobModal'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import {filterDictText} from '@/components/dict/JDictSelectUtil'
  import { httpAction } from '@/api/manage'

  export default {
    name: "SysJobList",
    mixins:[JeecgListMixin],
    components: {
      SysJobModal
    },
    data () {
      return {
        description: '岗位信息管理页面',
        jobList:[],
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
            title: '岗位名称',
            align:"center",
            dataIndex: 'jobName'
           },
		   {
            title: '排序',
            align:"center",
            dataIndex: 'jobOrder'
           },
		   {
            title: '对应职位',
            align:"center",
            dataIndex: 'jobPosition',
            customRender: (text) => {
               //字典值替换通用方法
               return filterDictText(this.jobList, text);
            }
           },
		   {
            title: '职位代码',
            align:"center",
            dataIndex: 'jobCode'
           },
		   {
            title: '理论工资',
            align:"center",
            dataIndex: 'jobPay'
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
          list: "/system/sysJob/list",
          delete: "/system/sysJob/delete",
          deleteBatch: "/system/sysJob/deleteBatch",
          exportXlsUrl: "system/sysJob/exportXls",
          importExcelUrl: "system/sysJob/importExcel",
          sysPositionList:"/system/sysPosition/getSysPositions",//查询职位
       },
    }
  },
  computed: {
    importExcelUrl: function(){
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
    }
  },
    methods: {
      initDictConfig() {
        //console.log("--我才是真的方法!--")
        httpAction(this.url.sysPositionList, { type: '' }, 'post').then((res)=>{
          if(res.success){
            this.jobList = res.result;
          }else{
            //console.log(res.message);
          }}
        );
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