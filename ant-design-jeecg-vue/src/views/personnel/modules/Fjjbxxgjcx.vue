<template>
  <a-modal
    title="高级查询"
    :width="1500"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @cancel="handleCancel"
    :mask="false"
    wrapClassName="ant-modal-cust-warp"
    style="top:5%;max-height: 95%;"
  >
    <template slot="footer">
      <a-button @click="handleCancel">关 闭</a-button>
      <a-button @click="searchReset" style="float: left">重 置</a-button>
      <a-button type="primary" @click="handleOk">查 询</a-button>
    </template>
    <a-spin :spinning="confirmLoading">
      <a-card :bordered="false">
        <!-- 查询区域 -->
        <div class="table-page-search-wrapper">
          <a-form layout="inline">
            <a-row :gutter="24">
              <a-col :md="6" :sm="8" v-for="(item,index) in fifleList" :key="index">
                <a-form-item :label="item.titile">
                  <a-input
                    placeholder
                    v-model="item.defaultvalue"
                    v-show="item.paramtype == 'input'"
                  ></a-input>
                  <a-textarea
                    v-model="item.defaultvalue"
                    placeholder
                    :autoSize="true"
                    v-show="item.paramtype == 'text'"
                  />
                  <a-input
                    type="number"
                    placeholder
                    v-model="item.defaultvalue"
                    v-show="item.paramtype == 'number'"
                  ></a-input>
                  <a-select v-model="item.defaultvalue" v-show="item.paramtype == 'select'">
                    <a-select-option
                      v-for="(it,one) in item.optionlist"
                      :key="one"
                      :value="it.value"
                    >{{it.value}}</a-select-option>
                  </a-select>
                  <a-date-picker
                    style="width:100%"
                    :format="item.dateformat"
                    v-if="item.defaultvalue==''||item.defaultvalue==null"
                    v-show="item.paramtype == 'date'"
                    @change="(date, dateString)=>{ item.defaultvalue =  dateString }"
                  />
                  <a-date-picker
                    style="width:100%"
                    :format="item.dateformat"
                    v-else
                    :value=" moment( item.defaultvalue, item.dateformat) "
                    v-show="item.paramtype == 'date'"
                    @change="(date, dateString)=>{ item.defaultvalue =  dateString }"
                  />
                  <a-month-picker
                    style="width:100%"
                    :format="item.dateformat"
                    v-if="item.defaultvalue==''||item.defaultvalue==null"
                    v-show="item.paramtype == 'monthDate'"
                    @change="(date, dateString)=>{ item.defaultvalue =  dateString}"
                  />
                  <a-month-picker
                    style="width:100%"
                    :format="item.dateformat"
                    v-else
                    :value=" moment( item.defaultvalue, item.dateformat) "
                    v-show="item.paramtype == 'monthDate'"
                    @change="(date, dateString)=>{ item.defaultvalue =  dateString}"
                  />
                </a-form-item>
              </a-col>
            </a-row>
          </a-form>
        </div>
      </a-card>
    </a-spin>
  </a-modal>
</template>

<script>
import { JeecgListMixin } from '@/mixins/chaxu'
import moment from 'moment'
import { deleteAction, getAction, downFile } from '@/api/manage'

export default {
  name: 'Fjjbxxgjcx',
  mixins: [JeecgListMixin],
  components: {},
  data() {
    return {
      visible: false,
      confirmLoading: false,
      monthFormat: 'YYYY-MM',
      fifleList: [],
    }
  },
  props: {},
  methods: {
    moment,
    dakai() {
      this.getShuJv() //获取数据
      this.visible = true
    },
    //获取数据
    getShuJv() {

      if(this.fifleList.length > 0)
      {
        return
      }


      let that = this
      let obj = {
        type: '人事档案'
      }
      that.confirmLoading = true
      getAction('/parmedit/sysParmEdit/listall', obj)
        .then(res => {
          if (res.success == true) {
            that.fifleList = res.result.records
            // that.fzInfoList = res.result
          } else {
            that.$message.error(res.message)
          }
        })
        .finally(() => {
          that.confirmLoading = false
        })
    },
    handleOk() {
      this.close()
    },
    handleCancel() {
      this.visible = false
    },
    close() {
      var obj
      var youzhiList = []
      obj = '{ '

      for (let i = 0; i < this.fifleList.length; i++) {
        if (this.fifleList[i].defaultvalue != null) {
          //判断是否有值 有值得话就塞值
          youzhiList.push({ name: this.fifleList[i].name, defaultvalue: this.fifleList[i].defaultvalue })
        }
      }
      for (let j = 0; j < youzhiList.length; j++) {
        if (j == youzhiList.length - 1) {
          obj += `"` + youzhiList[j].name + `":"` + youzhiList[j].defaultvalue + `"`
        } else {
          obj += `"` + youzhiList[j].name + `":"` + youzhiList[j].defaultvalue + `",`
        }
      }
      obj = obj + '}'
      console.log(JSON.parse(obj))
      this.$emit('close', JSON.parse(obj))
      this.visible = false
    },
    searchReset() {
      for (let i = 0; i < this.fifleList.length; i++) {
        this.fifleList[i].defaultvalue = ''
      }
    }
  }
}
</script>

<style >
</style>