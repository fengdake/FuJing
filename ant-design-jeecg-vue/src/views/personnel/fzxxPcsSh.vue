<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="24">
          <a-col :md="6" :sm="8">
            <a-form-item label="姓名">
              <a-input placeholder="请输入姓名" v-model="queryParam.xm"></a-input>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="身份证号">
              <a-input placeholder="请输入身份证号" v-model="queryParam.sfzh"></a-input>
            </a-form-item>
          </a-col>
          <template v-if="toggleSearchStatus">
            <a-col :md="6" :sm="8">
              <a-form-item label="辅警编号">
                <a-input placeholder="请输入辅警编号" v-model="queryParam.fjbh"></a-input>
              </a-form-item>
            </a-col>
            <a-col :md="6" :sm="8">
              <a-form-item label="所属单位">
                <a-select showSearch v-model="queryParam.ssdw" placeholder="请选择所在单位">
                  <a-select-option v-for="(item,index) in ssdwlist" :key="index" :value="item.text">
                    <span
                      style="display: inline-block;width: 100%"
                      :title=" item.text || item.label "
                    >{{ item.text || item.label }}</span>
                  </a-select-option>
                </a-select>
              </a-form-item>
            </a-col>
            <a-col :md="6" :sm="8">
              <a-form-item label="级别">
                <a-select v-model="queryParam.jb">
                  <a-select-option value="实习辅警">实习辅警</a-select-option>
                  <a-select-option value="四级辅警">四级辅警</a-select-option>
                  <a-select-option value="三级辅警">三级辅警</a-select-option>
                  <a-select-option value="二级辅警">二级辅警</a-select-option>
                  <a-select-option value="一级辅警">一级辅警</a-select-option>
                  <a-select-option value="副辅警长">副辅警长</a-select-option>
                  <a-select-option value="辅警长">辅警长</a-select-option>
                </a-select>
              </a-form-item>
            </a-col>
            <a-col :md="6" :sm="8">
              <a-form-item label="是否发放">
                <a-select v-model="queryParam.sfff">
                  <a-select-option value="是">是</a-select-option>
                  <a-select-option value="否">否</a-select-option>
                </a-select>
              </a-form-item>
            </a-col>

            <!-- <a-col :md="6" :sm="8">
              <a-form-item label="发放次数">
                <a-input placeholder="" v-model="queryParam.fjbh"></a-input>
              </a-form-item>
            </a-col>-->
            <!-- <a-col :md="6" :sm="8">
              <a-form-item label="档案出生年月">
                <a-month-picker
                  style="width:100%;"
                  placeholder="请选择档案出生年月"
                  :format="monthFormat"
                  v-model="queryParam.dacsny_begin"
                />
              </a-form-item>
            </a-col>-->
            <!-- <a-col :md="6" :sm="8">
              <a-form-item label="身份证出生年月">
                <a-month-picker
                  style="width:100%;"
                  placeholder="请选择身份证出生年月"
                  :format="monthFormat"
                  v-model="queryParam.dacsny_end"
                />
              </a-form-item>
            </a-col>-->
          </template>
          <a-col :md="6" :sm="8">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button
                type="primary"
                ghost
                @click="searchReset"
                icon="reload"
                style="margin-left: 8px"
              >重置</a-button>
              <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? '收起' : '展开' }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'" />
              </a>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button type="primary" icon="audit" @click="shenHe">一键审核</a-button>
      <!-- <a-button type="primary" icon="download" class="moBan" @click="handleExportXls('服装信息')">导出</a-button> -->
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel">
            <a-icon type="delete" />删除
          </a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px">
          批量操作
          <a-icon type="down" />
        </a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择
        <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
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
        :scroll="{ x: 1500}"
        @change="handleTableChange"
      >
        <span slot="action" slot-scope="text, record">
          <a @click="handleShenHe(record)">审核</a>
        </span>
      </a-table>
    </div>
    <!-- table区域-end -->
    <!-- 一键审核页面 -->
    <fzxxsh ref="zsh" :sh="sh" @modalFormOk="modalFormOk"></fzxxsh>
    <!-- 新增服装信息 -->
    <addfzxxsh ref="fzxxsh" :sh="sh" @modalFormOk="modalFormOk"></addfzxxsh>
    <!-- 编辑审核页面 -->
    <editfzxxsh ref="editfzxxsh" :sh="sh" @modalFormOk="modalFormOk"></editfzxxsh>
  </a-card>
</template>

<script>
import addfzxxsh from './modules/addfzxxsh'
import editfzxxsh from './modules/editfzxxsh'
import fzxxsh from './modules/fzxxsh'
import { ajaxGetSelectItems } from '@/api/api'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'
import { DisabledAuthFilterMixin } from '@/mixins/DisabledAuthFilterMixin'

export default {
  name: 'FzxxList',
  mixins: [JeecgListMixin, DisabledAuthFilterMixin],
  components: {
    addfzxxsh,
    editfzxxsh,
    fzxxsh
  },
  data() {
    return {
      description: '服装信息管理页面',
      monthFormat: 'YYYY-MM',
      ssdwlist: [],
      sh: '派出所审核',
      // 表头
      columns: [
        {
          title: '序号',
          dataIndex: '',
          key: 'rowIndex',
          width: 50,
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
          fixed: 'left',
          width: 100
        },
        {
          title: '性别',
          align: 'center',
          dataIndex: 'xb',
          fixed: 'left',
          width: 50
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
          title: '身份证号',
          align: 'center',
          dataIndex: 'sfzh'
        },
        {
          title: '身高(cm)',
          align: 'center',
          dataIndex: 'sg'
        },
        {
          title: '体重(KG)',
          align: 'center',
          dataIndex: 'tz'
        },
        {
          title: '头围(cm)',
          align: 'center',
          dataIndex: 'tw'
        },
        {
          title: '腰围(cm)',
          align: 'center',
          dataIndex: 'yw'
        },
        {
          title: '鞋码',
          align: 'center',
          dataIndex: 'xm2'
        },
        {
          title: '发放品种',
          align: 'center',
          dataIndex: 'ffpz'
        },
        {
          title: '是否发放',
          align: 'center',
          dataIndex: 'sfff'
        },
        {
          title: '审核结果',
          align: 'center',
          dataIndex: 'spjg'
        },
        {
          title: '操作',
          dataIndex: 'action',
          align: 'center',
          width: 100,
          fixed: 'right',
          scopedSlots: { customRender: 'action' }
        }
      ],
      url: {
        list: '/rsda/fzxx/pcsList',
        delete: '/rsda/fzxx/delete',
        deleteBatch: '/rsda/fzxx/deleteBatch',
        exportXlsUrl: 'rsda/fzxxSpjl/exportXls?spjg=派出所审核',
        importExcelUrl: 'rsda/fzxx/importExcel'
      }
    }
  },
  created() {
    //获取字典数据
    this.initDictData()
  },
  computed: {
    importExcelUrl: function() {
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`
    }
  },
  methods: {
    initDictData() {
      ajaxGetSelectItems('znpt_data.sys_depart,depart_name,org_code,org_code', null).then(res => {
        if (res.success) {
          this.ssdwlist = res.result
        }
      })
    },
    modalFormOk() {
      this.loadData()
    },
    //一键审核
    shenHe() {
      this.$refs.zsh.edit('总')
      this.$refs.zsh.title = '审核'
    },
    //审核
    handleShenHe(record) {
      if (record.fzxxSfzh == '' || record.fzxxSfzh == null) {
        this.$refs.fzxxsh.edit(record)
        this.$refs.fzxxsh.title = '审核'
      } else {
        this.$refs.editfzxxsh.edit(record)
        this.$refs.editfzxxsh.title = '审核'
      }
    }
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