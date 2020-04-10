<template>
  <a-modal
    :title="title"
    :width="1200"
    :visible="visible"
    :maskClosable="true"
    :confirmLoading="confirmLoading"
    :footer="null"
    @cancel="handleCancel">
    <a-spin :spinning="confirmLoading">
      <!-- 主表单区域 -->
      <a-form :form="form">
        <a-row>
          <a-col :span="12" :gutter="8">
            <a-form-item
              :labelCol="labelCol"
              :wrapperCol="wrapperCol"
              label="职位名称">
              <a-input placeholder="职位名称" v-decorator="['positionName', validatorRules.positionName ]" disabled/>
            </a-form-item>
          </a-col>
          <a-col :span="12" :gutter="8">
            <a-form-item
              :labelCol="labelCol"
              :wrapperCol="wrapperCol"
              label="部门类型">
              <a-select v-decorator="[ 'positionType', validatorRules.positionType]" placeholder="选择部门类型" disabled>
                <a-select-option value="1">总行</a-select-option>
                <a-select-option value="2">支行</a-select-option>
                <a-select-option value="3">机关部室</a-select-option>
                <a-select-option value="4">其他</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :span="12" :gutter="8">
            <a-form-item
              :labelCol="labelCol"
              :wrapperCol="wrapperCol"
              label="职位编码">
              <a-input placeholder="职位编码" v-decorator="['positionCode', validatorRules.positionCode ]" disabled/>
            </a-form-item>
          </a-col>
        </a-row>
      </a-form>

      <!-- 子表单区域 -->
      <a-tabs v-model="activeKey" @change="handleChangeTabs">
        <a-tab-pane tab="岗位" :key="refKeys[0]" :forceRender="true">
          <j-editable-table
            :chakan="true"
            :ref="refKeys[0]"
            :loading="sysRoTable.loading"
            :columns="sysRoTable.columns"
            :dataSource="sysRoTable.dataSource"
            :maxHeight="300"
            :rowNumber="true"
            :rowSelection="false"
            :actionButton="false"/>
        </a-tab-pane>
      </a-tabs>

    </a-spin>
  </a-modal>
</template>

<script>

  import moment from 'moment'
  import pick from 'lodash.pick'
  import { FormTypes } from '@/utils/JEditableTableUtil'
  import { JEditableTableMixin } from '@/mixins/JEditableTableMixin'

  export default {
    name: 'SysPosModal',
    mixins: [JEditableTableMixin],
    data() {
      return {
        // 新增时子表默认添加几行空数据
        addDefaultRowNum: 1,
        validatorRules: {
          positionName: { rules: [{ required: true, message: '请输入positionName!' }] },
          positionCode: { rules: [{ required: true, message: '请输入positionCode!' }] },
        },
        refKeys: ['sysRo', ],
        activeKey: 'sysRo',
        // 岗位
        sysRoTable: {
          loading: false,
          dataSource: [],
          columns: [
            {
              title: '岗位名称',
              key: 'roleName',
              type: FormTypes.input,
              defaultValue: '',
              placeholder: '请输入${title}',
            },
            {
              title: '岗位编码',
              key: 'roleCode',
              type: FormTypes.input,
              defaultValue: '',
              placeholder: '请输入${title}',
              validateRules: [{ required: true, message: '${title}不能为空' }],
            },
            {
              title: '描述信息',
              key: 'description',
              type: FormTypes.input,
              defaultValue: '',
              placeholder: '请输入${title}',
            },
          ]
        },
        url: {
          add: "/business/sysPos/add",
          edit: "/business/sysPos/edit",
          sysRo: {
            list: '/business/sysPos/querySysRoByMainId'
          },
        }
      }
    },
    methods: {
 
      /** 调用完edit()方法之后会自动调用此方法 */
      editAfter() {
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model, 'positionName', 'positionOrder', 'positionType', 'positionCode', 'status', 'delFlag', 'description', ))
          // 时间格式化
        })
        // 加载子表数据
        if (this.model.id) {
          let params = { id: this.model.id }
          this.requestSubTableData(this.url.sysRo.list, params, this.sysRoTable)
        }
      },
 
      /** 整理成formData */
      classifyIntoFormData(allValues) {
        let main = Object.assign(this.model, allValues.formValue)
        //时间格式化
        return {
          ...main, // 展开
          sysRoList: allValues.tablesValue[0].values,
        }
      }
    }
  }
</script>

<style scoped>
</style>