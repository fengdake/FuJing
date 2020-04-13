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
              <a-form-item label="审核状态">
                <a-select v-model="queryParam.spjg">
                  <a-select-option value="未审核">未审核</a-select-option>
                  <a-select-option value="派出所审核">派出所审核</a-select-option>
                  <a-select-option value="总局审核">总局审核</a-select-option>
                </a-select>
              </a-form-item>
            </a-col>
            <a-col :md="6" :sm="8">
              <a-form-item label="工资年月">
                <a-range-picker
                  :placeholder="['开始日期', '结束日期']"
                  format="YYYY-MM"
                  :value="[queryParam.gzksny,queryParam.gzjsny]"
                  :mode="mode2"
                  @panelChange="handlePanelChange2"
                  @change="handleChange"
                />
              </a-form-item>
            </a-col>
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
      <a-button @click="handleAdd" v-has="'cj:fj'" type="primary" icon="plus">创建月份</a-button>
      <a-button ghost @click="handleTijiao" v-has="'tj:fj'" type="primary" icon="check-circle">提交</a-button>
      <a-upload
        v-has="'dr:fj'"
        name="file"
        :showUploadList="false"
        :multiple="false"
        :headers="tokenHeader"
        :action="importExcelUrl"
        @change="handleImportExcel"
      >
        <a-button icon="import" class="daoru">导入</a-button>
      </a-upload>
      <a-button
        type="primary"
        v-has="'daoChu:fj'"
        class="moBan"
        icon="download"
        @click="handleExportXls('辅警工资信息')"
      >导出</a-button>
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
        <a style="font-weight: 600">
          {{
          selectedRowKeys.length }}
        </a>项
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
        @change="handleTableChange"
        :scroll="{ x: 3000}"
      >
        <template
          v-for="col in ['bxjs','bxjnqk','jbgz','jkxKhjx','jkxZqbz','jkxQt','kkxKhkk','kkxGrbx','kkxQt']"
          :slot="col"
          slot-scope="text, record, index"
        >
          <div :key="col" v-if="col !=='bxjnqk' ">
            <a-input
              v-has="'kt:fj'"
              style="margin: -5px 0"
              :value="text"
              @change="e => handleChangeA(e.target.value, record.id, col)"
            />
            <span v-has="'not:fj'">{{text}}</span>
          </div>
          <div :key="col" v-else>
            <a-select
              v-has="'kt:fj'"
              style="margin: -5px 0;width:100%;"
              :value="text"
              @change="e =>handleChanges(e,index)"
            >
              <a-select-option value="正常五险">正常五险</a-select-option>
              <a-select-option value="未参保">未参保</a-select-option>
              <a-select-option value="其他">其他</a-select-option>
            </a-select>
            <span v-has="'not:fj'">{{text}}</span>
          </div>
        </template>
        <span slot="action" slot-scope="text, record">
          <a @click="handleBaocun(record)" :disabled="disabledAuth('cz:fj')">保存</a>
          <a-divider type="vertical" />
          <a-popconfirm
            title="确定删除吗?"
            :disabled="disabledAuth('cz:fj')"
            @confirm="() => handleDelete(record.id)"
          >
            <a>删除</a>
          </a-popconfirm>
        </span>
      </a-table>
    </div>
    <!-- table区域-end -->

    <!-- 表单区域 -->
    <fjgzxx-modal ref="modalForm" @ok="modalFormOk"></fjgzxx-modal>
    <jcny ref="jiancha" @ok="modalFormOk"></jcny>
    <tj ref="tijiao" @ok="modalFormOk"></tj>
  </a-card>
</template>

<script>
import {ajaxGetSelectItems} from '@/api/api'
import { postAction, putAction, getAction } from '@/api/manage'
import FjgzxxModal from './modules/FjgzxxModal'
import jcny from './modules/cjny'
import tj from './modules/tj'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'
import { DisabledAuthFilterMixin } from '@/mixins/DisabledAuthFilterMixin'

export default {
  name: 'FjgzxxList',
  mixins: [JeecgListMixin, DisabledAuthFilterMixin],
  components: {
    FjgzxxModal,
    jcny,
    tj
  },
  data() {
    return {
      description: '辅警工资信息管理页面',
      monthFormat: 'YYYY-MM',
      mode2: ['month', 'month'],
      value: [],
      ssdwlist: [],
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
          title: '减少时间',
          align: 'center',
          dataIndex: 'jsrq'
        },
        {
          title: '银行卡号',
          align: 'center',
          width: 180,
          dataIndex: 'yhkh',
          scopedSlots: { customRender: 'yhkh' }
        },
        {
          title: '工资发放银行',
          align: 'center',
          dataIndex: 'gzffyh',
          scopedSlots: { customRender: 'gzffyh' }
        },
        {
          title: '保险基数',
          align: 'center',
          dataIndex: 'bxjs',
          scopedSlots: { customRender: 'bxjs' }
        },
        {
          title: '保险缴纳情况',
          align: 'center',
          dataIndex: 'bxjnqk',
          width: 200,
          scopedSlots: { customRender: 'bxjnqk' }
        },
        {
          title: '基本工资',
          align: 'center',
          dataIndex: 'jbgz',
          scopedSlots: { customRender: 'jbgz' }
        },
        {
          title: '加款项:考核绩效（元）',
          align: 'center',
          dataIndex: 'jkxKhjx',
          scopedSlots: { customRender: 'jkxKhjx' }
        },
        {
          title: '加款项:执勤补助（元）',
          align: 'center',
          dataIndex: 'jkxZqbz',
          scopedSlots: { customRender: 'jkxZqbz' }
        },
        {
          title: '加款项:其他（元）',
          align: 'center',
          dataIndex: 'jkxQt',
          scopedSlots: { customRender: 'jkxQt' }
        },
        {
          title: '扣款项:考核扣款（元）',
          align: 'center',
          dataIndex: 'kkxKhkk',
          scopedSlots: { customRender: 'kkxKhkk' }
        },
        {
          title: '扣款项:个人保险（元）',
          align: 'center',
          dataIndex: 'kkxGrbx',
          scopedSlots: { customRender: 'kkxGrbx' }
        },
        {
          title: '扣款项:其他（元）',
          align: 'center',
          dataIndex: 'kkxQt',
          scopedSlots: { customRender: 'kkxQt' }
        },
        {
          title: '实发数（元）',
          align: 'center',
          dataIndex: 'sfs',
          scopedSlots: { customRender: 'sfs' }
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
      url: {
        list: '/rsda/fjgzxx/list',
        delete: '/rsda/fjgzxx/delete',
        deleteBatch: '/rsda/fjgzxx/deleteBatch',
        exportXlsUrl: 'rsda/fjgzxx/exportXls',
        importExcelUrl: 'rsda/fjgzxx/importExcel'
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
    handleChange(value) {
      this.queryParam.gzksny = value[0]
      this.queryParam.gzjsny = value[1]
    },
    handlePanelChange2(value, mode) {
      this.queryParam.gzksny = value[0]
      this.queryParam.gzjsny = value[1]
      this.mode2 = [mode[0] === 'date' ? 'month' : mode[0], mode[1] === 'date' ? 'month' : mode[1]]
    },
    handleChanges(e, index) {
      this.dataSource[index].bxjnqk = e
    },
    handleChangeA(value, key, column) {
      const newData = this.dataSource
      const target = newData.filter(item => key === item.id)[0]
      if (target) {
        target[column] = value
        target.sfs =Number(target.jbgz) + Number(target.jkxKhjx) + Number(target.jkxZqbz) + Number(target.jkxQt) - Number(target.kkxKhkk) - Number(target.kkxGrbx) - Number(target.kkxQt)
        console.log(target.sfs)
        this.dataSource = newData
      }
    },
    //提交
    handleTijiao() {
      this.$refs.tijiao.edit()
      this.$refs.tijiao.title = '提交'
    },
    //创建年月
    handleAdd() {
      this.$refs.jiancha.edit()
      this.$refs.jiancha.title = '创建月份'
    },
    //保存数据
    handleBaocun(record) {
      console.log(record)
      let obj = {
        id: record.id,
        xm: record.xm, //姓名
        sfzh: record.sfzh, //身份证号
        gzny: record.gzny, //工资年月
        jsrq: record.jsrq,//减少时间
        yhkh: record.yhkh, //银行卡号
        gzffyh: record.gzffyh, //工资发放银行
        bxjs: record.bxjs, //保险基数
        bxjnqk: record.bxjnqk, //保险缴纳情况
        jbgz: record.jbgz, //基本工资
        jkxKhjx: record.jkxKhjx, //加款项：考核绩效
        jkxZqbz: record.jkxZqbz, //加款项：执勤补助
        jkxQt: record.jkxQt, //价款项：其他
        kkxKhkk: record.kkxKhkk, //扣款项：考核扣款
        kkxGrbx: record.kkxGrbx, //扣款项：个人保险
        kkxQt: record.kkxQt, //扣款项：其他
        sfs: record.sfs //实发数
      }
      putAction('/rsda/fjgzxx/edit', obj)
        .then(res => {
          if (res.success == true) {
            this.$message.success(res.message)
            this.loadData()
          } else {
            this.$message.error(res.message)
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
.moBan {
  background: #17c295;
  border: 1px solid #17c295;
}
.moBan:hover {
  background: #3ad0a8;
  border: 1px solid #17c295;
}
.daoru {
  background: #ffffff;
  color: #17c295;
  border: 1px solid #17c295;
}
.daoru:hover {
  background: #ffffff;
  color: #3ad0a8;
  border: 1px solid #3ad0a8;
}
</style>