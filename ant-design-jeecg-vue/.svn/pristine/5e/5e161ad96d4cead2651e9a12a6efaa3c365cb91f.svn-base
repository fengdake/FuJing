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
          label="移动端系统版本">
          <a-input placeholder="请输入移动端系统版本" v-decorator="['mobileVersion', validatorRules.mobileVersion ]" />
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="更新标题">
          <a-input placeholder="请输入更新标题" v-decorator="['versionName', {}]" />
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="更新内容">
          <a-input placeholder="请输入更新内容" v-decorator="['updateContent', {}]" />
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="app大小">
          <a-input placeholder="请输入app大小" v-decorator="['updateSize', {}]" />
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="是否强制更新">
          <a-select placeholder="请选择是否强制更新" v-decorator="['isForceUpdate',{}]">
            <a-select-option value="1">启用</a-select-option>
            <a-select-option value="0">禁用</a-select-option>
          </a-select>
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="更新地址">
          <a-input disabled placeholder="请输入更新地址" v-decorator="['packageUrl', {}]" />
        </a-form-item>
        <a-form-item :labelCol="labelCol"
                     :wrapperCol="wrapperCol"
                     label="上传app">
        <a-upload
          action="/jeecg-boot/sys/common/upload"
          listType="picture-card"
          :fileList="fileList"
          @change="handleChange">
          <div v-if="fileList.length < 1">
            <a-icon type="plus" />
            <div class="ant-upload-text">APP</div>
          </div>
        </a-upload>
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
    name: "SysVersionModal",
    data () {
      return {
        fileList:[],
        title:"操作",
        visible: false,
        model: {},
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
        mobileVersion:{rules: [{ required: true, message: '请输入移动端系统版本!' }]},
        },
        url: {
          add: "/system/sysVersion/add",
          edit: "/system/sysVersion/edit",
        },
      }
    },
    created () {
    },
    methods: {
      add () {
        this.edit({});
      },
      edit (record) {
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.fileList = [];
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'mobileVersion','versionName','updateContent','updateSize','isForceUpdate','packageUrl'))
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
      handleChange ({ fileList }) {
        this.fileList = fileList
        if (this.fileList.length > 0)
        {
          if (this.fileList[0].response)
          {
            this.model.packageUrl = this.fileList[0].response.message;
            this.form.setFieldsValue(pick(this.model,'packageUrl'))
          }
        }
        else
        {
          this.model.packageUrl = "";
          this.form.setFieldsValue(pick(this.model,'packageUrl'))
        }
      }
    }
  }
</script>

<style lang="less" scoped>

</style>