<template>
  <a-modal
    :title="title"
    :width="800"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">
    
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">
      
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="岗位名称">
          <a-input placeholder="请输入岗位名称" v-decorator="['jobName', validatorRules.jobName ]" />
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="排序">
          <a-input-number v-decorator="[ 'jobOrder', {}]" />
        </a-form-item>
        <a-form-item label="对应职位" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-select
            style="width: 100%"
            placeholder="请选择对应职位" v-decorator="['jobPosition', {}]">
            <a-select-option  v-for="(item,index) in jobList" :key="index.toString()" :value="item.value">
              {{ item.text }}
            </a-select-option>
          </a-select>

        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="岗位代码">
          <a-input placeholder="请输入岗位代码" v-decorator="['jobCode', validatorRules.jobCode ]" />
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="理论工资">
          <a-input-number v-decorator="[ 'jobPay', {}]" />
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="选用状态">
          <a-select v-decorator="[ 'status', validatorRules.jobStatus]" placeholder="请选择选用状态">
            <a-select-option value="0">否</a-select-option>
            <a-select-option value="1">是</a-select-option>
          </a-select>
        </a-form-item>
      </a-form>
    </a-spin>
  </a-modal>
</template>

<script>
  import { httpAction } from '@/api/manage'
  import pick from 'lodash.pick'
  import moment from "moment"

  export default {
    name: "SysJobModal",
    data () {
      return {
        title:"操作",
        visible: false,
        model: {},
        jobList:[],
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },

        confirmLoading: false,
        form: this.$form.createForm(this),
        validatorRules:{
        jobName:{rules: [{ required: true, message: '请输入岗位名称!' }]},
        jobCode:{rules: [{ required: true, message: '请输入职位代码!' }]},
        jobStatus:{rules: [{ required: true, message: '请选择选用状态!' }]},
        },
        url: {
          add: "/system/sysJob/add",
          edit: "/system/sysJob/edit",
          sysPositionList:"/system/sysPosition/getSysPositions",//查询职位
        },
      }
    },
    created () {
    },
    methods: {
      initialJobList(){
        httpAction(this.url.sysPositionList, { type: '' }, 'post').then((res)=>{
          if(res.success){
            this.jobList = res.result;
          }else{
            //console.log(res.message);
          }
        });
      },
      add () {
        this.edit({});
      },
      edit (record) {
        let that = this;
        that.initialJobList();
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'jobName','jobOrder','jobPosition','jobCode','jobPay','status','delFlag','description'))
		  //时间格式化
        });

      },
      close () {
        this.$emit('close');
        this.visible = false;
      },
      handleOk () {
        const that = this;
        // 触发表单验证
        this.form.validateFields((err, values) => {
          if (!err) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }
            let formData = Object.assign(this.model, values);
            //时间格式化
            
            //console.log(formData)
            httpAction(httpurl,formData,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
              that.close();
            })



          }
        })
      },
      handleCancel () {
        this.close()
      },


    }
  }
</script>

<style lang="less" scoped>

</style>