<template>
  <a-modal
    :title="title"
    width="100%"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @cancel="handleCancel"
    style="top:0;overflow-y: auto;"
    cancelText="关闭"
    :footer="null"
  >
    <a-spin :spinning="confirmLoading">
      <div style="margin-bottom: 15px;text-align: right;">
        <a-button type="primary" icon="file-text" @click="tijiao">提交</a-button>
      </div>
      <div class="jiben">
        <div class="disflex1 chakan-a">
          <div class="geren-box">
            <div style="display: flex; align-items: center;margin-bottom: 20px;">
              <span class="xian"></span>
              <span class="title">个人信息</span>
              <span
                class="title"
                style="display: inline-block;margin-left: 20px;font-size: 14px;"
              >Personal Information</span>
              <img src="~@/assets/sui1.png" alt style="margin-left: 30px;" />
            </div>
            <div class="disflex space-between" style="flex-wrap: wrap;padding: 0 14px;">
              <div
                class="disflex xinxi-box"
                v-for="(item,index) in jibenInfoList.jbxx"
                :key="index"
              >
                <div class="xinXi-a">{{item.title}}:</div>
                <div class="xinXi-b">{{item.value}}</div>
              </div>
            </div>
          </div>
          <div class="touxiang-box">
            <img
              src="~@/assets/touxiang.png"
              v-if="jibenInfoList.txdz == '' || jibenInfoList.txdz == null"
              class="touxiang-img"
            />
            <img :src="lujing + jibenInfoList.txdz" v-else class="touxiang-img" alt />
            <div class="name">{{jibenInfoList.xm}}</div>
            <div class="jibie" v-show="jibenInfoList.jb != '' && jibenInfoList.jb != null ">
              <span class="jibie-a">{{jibenInfoList.jb}}</span>
            </div>
          </div>
        </div>
      </div>
      <jbxxsh ref="zsh" :sh="sh" :bs='bs' @close='close'></jbxxsh>
    </a-spin>
  </a-modal>
</template>

<script>
import { httpAction, postAction, putAction, getAction } from '@/api/manage'
import jbxxsh from './jbxxsh'
import pick from 'lodash.pick'
import moment from 'moment'
export default {
  name: 'chakanjbxx',
  components: {jbxxsh},
  data() {
    return {
      title: '操作',
      visible: false,
      model: {},
      bs: 'xz',
      confirmLoading: false,
      loading: false,
      imageUrl: '',
      dateFormat: 'YYYY-MM-DD',
      lujing: window._CONFIG['imgDomainURL'] + '/',
      jibenInfoList: {},
      shujv: {},
    }
  },
  props: ['sh'],
  created() {},
  methods: {
    moment,
    edit(record) {
      let that = this
      this.visible = true
      this.shujv = record
      this.chaxun(record.id) //查询基本信息
    },
    chaxun(id) {
      var that = this
      let obj = {
        id: id
      }
      this.confirmLoading = true
      getAction('/rsda/fjjbxxSpjl/queryInfo', obj)
        .then(res => {
          if (res.success == true) {
            this.jibenInfoList = res.result
          } else {
            that.$message.error(res.message)
          }
        })
        .finally(() => {
          this.confirmLoading = false
        })
    },
    //提交
    tijiao(){
        this.$refs.zsh.edit('单',this.shujv)
        this.$refs.zsh.title = '审核'
    },
    close() {
      this.visible = false
      this.$emit('Refresh')
    },
    handleCancel() {
      this.close()
    }
  }
}
</script>

<style lang="less" scoped>
.geren-box {
  width: 80%;
  margin: 0 30px;
  padding: 16px 10px;
}
.xian {
  display: inline-block;
  width: 3px;
  height: 16px;
  background-color: #0177d5;
  margin-right: 8px;
}
.title {
  font-size: 16px;
  color: #0177d5;
  font-weight: bold;
}
.jiben {
  position: relative;
  box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.15);
  border-radius: 5px;
  min-height: 100px;
  margin-bottom: 24px;
  border: solid 1px #eef3f6;
}
.disflex {
  display: flex;
  align-items: center;
}
.disflex1 {
  display: flex;
}
.space-between {
  justify-content: space-between;
}
.chakan-a {
  padding-top: 30px;
}
.xinxi-box {
  width: 40%;
  padding-top: 12px;
}
.xinXi-a {
  font-size: 12px;
  width: 35%;
}
.xinXi-b {
  width: 65%;
  font-size: 12px;
  color: black;
}
.touxiang-box {
  width: 40%;
  text-align: center;
  padding: 16px 10px;
}
.touxiang-img {
  width: 240px;
  height: 331px;
  border: solid 1px #eef3f6;
  display: inline-block;
  &:hover {
    cursor: pointer;
    border: dashed 1px #0177d5;
  }
}
.name {
  text-align: center;
  color: #0177d5;
  font-size: 14px;
  font-weight: bold;
  line-height: 40px;
}
.jibie {
  text-align: center;
}
.jibie-a {
  display: inline-block;
  padding: 3px 30px;
  color: #ffffff;
  background-color: brown;
  border-radius: 3px;
}
// .tupian {
//   width: 32px;
//   height: 32px;
//   margin-left: -10px;
//   z-index: 100;
//   margin-top: -1px;
// }
.diandian {
  display: inline-block;
  width: 16px;
  height: 16px;
  margin-left: -6px;
  z-index: 100;
  margin-top: -1px;
  border-radius: 50%;
  background-color: #51aafd;
}
.jinduc_b {
  width: 172px;
  height: 115px;
  border: 1px solid #1890ff;
  border-radius: 8px;
  background-color: #f3f9ff;
  margin-top: 10px;
}
.jinduc_a {
  width: 100%;
  color: #333333;
  font-size: 14px;
  display: flex;
}
.jinduc_ab {
  width: 100%;
  color: #adadad;
  font-size: 14px;
}
.jinduc {
  width: 90%;
  margin-left: 60px;
}
.jindua {
  display: flex;
}
.jindub {
  width: 3.5px;
  background-color: #51aafd;
}
.jindubjieshu {
  width: 3.5px;
  background-color: #51aafd;
  height: 80px;
}
.jindu_left {
  height: 85px;
  width: 300px;
  border: 1px solid #1890ff;
  border-radius: 10px;
  background-color: #f3f9ff;
  display: flex;
  flex-flow: column;
  justify-content: center;
  margin-left: 20px;
}
.jindu_left_weia {
  text-align: right;
  color: #adadad;
  font-size: 18px;
}
.jindu_left_wei {
  height: 85px;
  width: 300px;
  margin-left: 20px;
}
.moxingb_a {
  width: 90px;
  height: 41px;
  font-size: 16px;
  color: #ffffff;
  background-color: #46a5fe;
  border: 1px solid #46a5fe;
  border-radius: 8px;
  outline: none;
  cursor: pointer;
}
.moxingb_b {
  width: 90px;
  height: 41px;
  font-size: 16px;
  color: #46a5fe;
  background-color: #ffffff;
  border: 1px solid #46a5fe;
  border-radius: 8px;
  margin-left: 50px;
  outline: none;
  cursor: pointer;
}
.moxingb {
  margin-top: 34px;
  text-align: center;
}
.jieguo {
  display: flex;
  padding: 0px 12px;
  font-size: 18px;
}
.jieguoa {
  width: 176px;
  text-align: right;
  line-height: 40px;
}
.jieguob {
  display: flex;
  justify-content: space-between;
  width: 350px;
  align-items: center;
  margin-left: 20px;
}
.jieguoh {
  width: 100%;
  height: 100%;
  color: #49a0ed;
  background-color: rgba(73, 160, 237, 0.1);
  border: 1px solid #49a0ed;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 14px;
  height: 108px;
}
.jieguob img {
  width: 23px;
  margin-right: 5px;
}
.jieguob span {
  display: flex;
  align-items: center;
  cursor: pointer;
}
.time {
  display: inline-block;
  width: 77px;
  font-size: 14px;
  color: #666666;
}
.timevalue {
  font-size: 14px;
  color: #000000;
  padding: 0;
}
</style>