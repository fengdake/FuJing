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
      <div class="jiben">
        <div class="geren-box" style="justify-content: space-between;">
          <div style="display: flex; align-items: center;">
            <span class="xian"></span>
            <span class="title">个人信息</span>
          </div>
          <a-button type="primary" icon="file-text" @click="baocun">保存</a-button>
        </div>
        <div class="geRen-a">
          <div class="jibenb">
            <div class="jibenb_a" v-for="(item,index) in geRenInfo" :key="index">
              <span class="jibenb_a_name">
                <a v-show="item.require" style="font-size:20px;color:#ff4848;">*</a>
                {{item.name}}:
              </span>
              <a-input
                style="width:198px"
                :value="item.value"
                @change="(e)=>{ item.value = e.target.value }"
                v-show="item.status == 1 "
                :disabled="item.jinyong"
                :placeholder="item.placehold"
              />
              <a-select
                :placeholder="item.placehold"
                :dropdownMatchSelectWidth="false"
                v-model="item.value"
                v-show="item.status == 2 "
                style="width:198px"
                class="jibenb_a_select"
              >
                <a-select-option
                  v-for="(itea,ind) in item.second"
                  :key="ind"
                  :value="itea.name"
                >{{itea.name}}</a-select-option>
              </a-select>
              <a-month-picker
                style="width:198px"
                :format="dateFormat"
                v-if="item.value==''||item.value==null"
                v-show="item.status== 3 "
                @change="(date, dateString)=>{ item.value =  dateString }"
              />
              <a-month-picker
                style="width:198px"
                :format="dateFormat"
                v-else
                :value=" moment( item.value, dateFormat) "
                v-show="item.status== 3 "
                @change="(date, dateString)=>{ item.value =  dateString }"
              />
              <a-textarea
                style="width:597px;color:black"
                v-show="item.status== 4 "
                :value="item.value"
                @change="(e)=>{ item.value = e.target.value }"
                :autoSize="true"
              />
              <a-date-picker
                style="width:198px"
                :format="date"
                v-if="item.value==''||item.value==null"
                v-show="item.status== 5 "
                @change="(date, dateString)=>{ item.value =  dateString }"
              />
              <a-date-picker
                style="width:198px"
                :format="date"
                v-else
                :value=" moment( item.value, date) "
                v-show="item.status== 5 "
                @change="(date, dateString)=>{ item.value =  dateString }"
              />
              <a-input
                style="width:198px"
                :value="item.value"
                type="number"
                v-show="item.status == 9 "
                :placeholder="item.placehold"
                @change="(e)=>{ item.value = e.target.value }"
              />
            </div>
          </div>
          <div class="shangchuan-box">
            <a-upload
              name="file"
              listType="picture-card"
              class="avatar-uploader"
              :showUploadList="false"
              :action="fileUpload"
              @change="handleChange"
            >
              <img
                v-if="imageUrl"
                :src="lujing+imageUrl"
                alt="avatar"
                style="width: 240px;height: 331px;"
              />
              <div v-else>
                <a-icon :type="loading ? 'loading' : 'plus'" />
                <div class="ant-upload-text">请上传个人一寸照片</div>
              </div>
            </a-upload>
          </div>
        </div>
      </div>
      <div class="jiben">
        <div class="geren-box">
          <span class="xian"></span>
          <span class="title">奖惩信息</span>
        </div>
        <div style="padding: 0px 6px;">
          <jcxx ref="jcxxas" :gerenid="gerenid" :sfzh="geRenInfo[3].value"></jcxx>
        </div>
      </div>
      <div class="jiben">
        <div class="geren-box">
          <span class="xian"></span>
          <span class="title">个人简历</span>
        </div>
        <div style="padding: 0px 6px;">
          <grjl ref="childb" :gerenid="gerenid" :sfzh="geRenInfo[3].value"></grjl>
        </div>
      </div>
    </a-spin>
  </a-modal>
</template>

<script>
import { httpAction, postAction, putAction } from '@/api/manage'
import pick from 'lodash.pick'
import Vue from 'vue'
import { USER_INFO } from '@/store/mutation-types'
import moment from 'moment'
import jcxx from '../JcxxList'
import grjl from '../GrjlList'
function getBase64(img, callback) {
  const reader = new FileReader()
  reader.addEventListener('load', () => callback(reader.result))
  reader.readAsDataURL(img)
}
export default {
  name: 'FjJbxxModal',
  components: {
    jcxx,
    grjl
  },
  data() {
    return {
      title: '操作',
      visible: false,
      model: {},
      labelCol: {
        xs: { span: 24 },
        sm: { span: 5 }
      },
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 16 }
      },

      confirmLoading: false,
      loading: false,
      imageUrl: '',
      dateFormat: 'YYYY-MM',
      date: 'YYYY-MM-DD',
      lujing: window._CONFIG['imgDomainURL'] + '/',
      fileUpload: window._CONFIG['domianURL'] + '/sys/common/upload',
      gerenid: '', //个人id
      geRenInfo: [
        {
          name: '姓名',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '性别',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '2',
          second: [{ name: '男' }, { name: '女' }]
        },
        {
          name: '民族',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '身份证号码',
          require: true,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '库别',
          require: true,
          jinyong: false,
          placehold: '',
          value: '',
          status: '2',
          second: [
            { name: '在职' },
            { name: '（减少）退休' },
            { name: '（减少）辞职' },
            { name: '（减少）辞退' },
            { name: '（减少）死亡' }
          ]
        },
        {
          name: '减少时间',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '5',
          second: []
        },
        {
          name: '档案出生年月',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '3',
          second: []
        },
        {
          name: '身份证出生年月',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '3',
          second: []
        },
        {
          name: '级别',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '2',
          second: [
            { name: '实习辅警' },
            { name: '四级辅警' },
            { name: '三级辅警' },
            { name: '二级辅警' },
            { name: '一级辅警' },
            { name: '副辅警长' },
            { name: '辅警长' }
          ]
        },
        {
          name: '辅警编号',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '所属单位',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '学历',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '2',
          second: [
            { name: '研究生' },
            { name: '大学本科' },
            { name: '大学专科' },
            { name: '中专/中等技校/高中' },
            { name: '初中' },
            { name: '初中以下' },
            { name: '缺失' }
          ]
        },
        {
          name: '学位',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '2',
          second: [
            { name: '名誉博士' },
            { name: '博士' },
            { name: '大学专科' },
            { name: '硕士' },
            { name: '学士' },
            { name: '无' }
          ]
        },
        {
          name: '政治面貌',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '2',
          second: [
            { name: '中共党员' },
            { name: '中共预备党员' },
            { name: '共青团员' },
            { name: '民革党员' },
            { name: '民盟盟员' },
            { name: '民建会员' },
            { name: '民进会员' },
            { name: '农工党党员' },
            { name: '致公党党员' },
            { name: '九三学社社员' },
            { name: '台盟盟员' },
            { name: '群众' },
            { name: '无党派人士' }
          ]
        },
        {
          name: '入党时间',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '3',
          second: []
        },
        {
          name: '所在党支部',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '是否服役',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '2',
          second: [{ name: '是' }, { name: '否' }]
        },
        {
          name: '退伍时间',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '3',
          second: []
        },
        {
          name: '毕业院校',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '所学专业',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '参加公安工作时间',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '3',
          second: []
        },
        {
          name: '参加工作时间',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '3',
          second: []
        },
        {
          name: '就业登记证号码',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '签订合同日期',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '5',
          second: []
        },
        {
          name: '合同截止日期',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '5',
          second: []
        },
        {
          name: '签订合同类别',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '2',
          second: [{ name: '固定期限' }, { name: '无固定期限' }]
        },
        {
          name: '签订合同次数',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '身份性质',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '2',
          second: [
            { name: '辅警（保安队员）' },
            { name: '辅警（社区辅警）' },
            { name: '辅警（机关辅警）' },
            { name: '辅警（巡特警队员）' },
            { name: '辅警（交通协管员）' },
            { name: '辅警（调解员）' },
            { name: '辅警（借调人员）' },
            { name: '企业工人' },
            { name: '事业工人' },
            { name: '其他' },
            { name: '财拨工人' }
          ]
        },
        {
          name: '招录方式',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '2',
          second: [{ name: '公开招录' }, { name: '推荐招录' }]
        },
        {
          name: '招聘人',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '辅警类别',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '2',
          second: [{ name: '文职辅警' }, { name: '勤务辅警' }]
        },
        {
          name: '银行卡号',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '开户行',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '1',
          second: []
        },
        {
          name: '联系电话',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '9',
          second: []
        },
        {
          name: '户籍地址',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '4',
          second: []
        },
        {
          name: '常住地址',
          require: false,
          jinyong: false,
          placehold: '',
          value: '',
          status: '4',
          second: []
        }
      ],
      url: {
        add: '/rsda/fjJbxx/add',
        edit: '/rsda/fjJbxx/edit'
      }
    }
  },
  created() {},
  methods: {
    moment,
    handleChange(info) {
      console.log(info)
      if (info.file.status === 'uploading') {
        this.loading = true
        return
      }
      if (info.file.status === 'done') {
        // Get this url from response in real world.
        console.log(info.file.originFileObj)
        // getBase64(info.file.originFileObj, imageUrl => {
        this.imageUrl = info.file.response.message
        this.loading = false
        // })
      }
    },
    beforeUpload(file) {
      const isJpgOrPng = file.type === 'image/jpeg' || file.type === 'image/png'
      if (!isJpgOrPng) {
        this.$message.error('You can only upload JPG file!')
      }
      const isLt2M = file.size / 1024 / 1024 < 2
      if (!isLt2M) {
        this.$message.error('Image must smaller than 2MB!')
      }
      return isJpgOrPng && isLt2M
    },
    add() {
      this.edit({ ssdw: Vue.ls.get(USER_INFO).orgName })
    },
    edit(record) {
      console.log(record)
      let that = this
      if (record == '') {
        for (var i = 0; i < this.geRenInfo.length; i++) {
          this.geRenInfo[i].value = ''
        }
        this.gerenid = ''
        this.imageUrl = ''
      } else {
        this.fuzhi(record)
      }
      this.visible = true
      setTimeout(function() {
        that.$refs.jcxxas.shuaxin()
        that.$refs.childb.sing()
      }, 80)
    },
    close() {
      this.$emit('Refresh')
      this.visible = false
    },
    //赋值
    fuzhi(record) {
      this.gerenid = record.id
      this.geRenInfo[0].value = record.xm //姓名
      this.geRenInfo[1].value = record.xb //性别
      this.geRenInfo[2].value = record.mz //民族
      this.geRenInfo[3].value = record.sfzh //身份证号码
      this.geRenInfo[4].value = record.kb //库别
      this.geRenInfo[5].value = record.jsrq//减少时间
      this.geRenInfo[6].value = record.dacsny //档案出生年月
      this.geRenInfo[7].value = record.sfzcsny //身份证出生年月
      this.geRenInfo[8].value = record.jb //级别
      this.geRenInfo[9].value = record.fjbh //辅警编号
      this.geRenInfo[10].value = record.ssdw //所属单位
      this.geRenInfo[11].value = record.xl //学历
      this.geRenInfo[12].value = record.xw //学位
      this.geRenInfo[13].value = record.zzmm //政治面貌
      this.geRenInfo[14].value = record.rdsj //入党时间
      this.geRenInfo[15].value = record.szdzb //所在党支部
      this.geRenInfo[16].value = record.sffy //是否服役
      this.geRenInfo[17].value = record.twsj //退伍时间
      this.geRenInfo[18].value = record.byyx //毕业院校
      this.geRenInfo[19].value = record.sxzy //所学专业
      this.geRenInfo[20].value = record.cjgagzsj //参加公安工作时间
      this.geRenInfo[21].value = record.cjgzsj //参加工作时间
      this.geRenInfo[22].value = record.jydjzhm //就业登记证号码
      this.geRenInfo[23].value = record.qdhtrq //签订合同日期
      this.geRenInfo[24].value = record.htjzrq //合同截止日期
      this.geRenInfo[25].value = record.qdhtlb //签订合同类别
      this.geRenInfo[26].value = record.qdhtcs //签订合同次数
      this.geRenInfo[27].value = record.sfxz //身份性质
      this.geRenInfo[28].value = record.zlfs //招录方式
      this.geRenInfo[29].value = record.zpr //招聘人
      this.geRenInfo[30].value = record.fjlb //辅警类别
      this.geRenInfo[31].value = record.yhkh //银行卡号
      this.geRenInfo[32].value = record.khh //开户行
      this.geRenInfo[33].value = record.lxdh //联系电话
      this.geRenInfo[34].value = record.hjdz //户籍地址
      this.geRenInfo[35].value = record.czdz //常住地址
      this.imageUrl = record.txdz //头像地址
    },
    //保存个人信息
    baocun() {
      var that = this
      for (var i = 0; i < this.geRenInfo.length; i++) {
        if (this.geRenInfo[i].require == true) {
          if (this.geRenInfo[i].value == '' || this.geRenInfo[i].value == null) {
            this.$message.warning('请输入' + this.geRenInfo[i].name + '!')
            return
          }
        }
      }
      //判断库别是否在职
      if(this.geRenInfo[4].value != '在职'){
        if(this.geRenInfo[5].value == '' || this.geRenInfo[5].value == null){
          this.$message.warning('请选择减少时间!')
        return
        }
      }
      if (that.IdentityCodeValid(this.geRenInfo[3].value) == false) {
        this.$message.warning('请先输入正确身份证号码!')
        return
      }
      if (that.geRenInfo[32].value != '' && that.geRenInfo[32].value != null) {
        var phone = /^1[1234567890]\d{9}$/
        // var guHua = /^((\d{7,8})|(\d{4}|\d{3})-(\d{7,8})|(\d{4}|\d{3})-(\d{7,8})-(\d{4}|\d{3}|\d{2}|\d{1})|(\d{7,8})-(\d{4}|\d{3}|\d{2}|\d{1}))$/
        if (!phone.test(that.geRenInfo[32].value)) {
          that.$message.warning('请输入正确的联系电话！')
          return
        }
      }
      this.confirmLoading = true
      if (this.gerenid == '' || this.gerenid == null) {
        let obj = {
          xm: this.geRenInfo[0].value, //姓名
          xb: this.geRenInfo[1].value, //性别
          mz: this.geRenInfo[2].value, //民族
          sfzh: this.geRenInfo[3].value, //身份证号码
          kb: this.geRenInfo[4].value, //库别
          jsrq:this.geRenInfo[5].value,//减少时间
          dacsny: this.geRenInfo[6].value, //档案出生年月
          sfzcsny: this.geRenInfo[7].value, //身份证出生年月
          jb: this.geRenInfo[8].value, //级别
          fjbh: this.geRenInfo[9].value, //辅警编号
          ssdw: this.geRenInfo[10].value, //所属单位
          xl: this.geRenInfo[11].value, //学历
          xw: this.geRenInfo[12].value, //学位
          zzmm: this.geRenInfo[13].value, //政治面貌
          rdsj: this.geRenInfo[14].value, //入党时间
          szdzb: this.geRenInfo[15].value, //所在党支部
          sffy: this.geRenInfo[16].value, //是否服役
          twsj: this.geRenInfo[17].value, //退伍时间
          byyx: this.geRenInfo[18].value, //毕业院校
          sxzy: this.geRenInfo[19].value, //所学专业
          cjgagzsj: this.geRenInfo[20].value, //参加公安工作时间
          cjgzsj: this.geRenInfo[21].value, //参加工作时间
          jydjzhm: this.geRenInfo[22].value, //就业登记证号码
          qdhtrq: this.geRenInfo[23].value, //签订合同日期
          htjzrq: this.geRenInfo[24].value, //合同截止日期
          qdhtlb: this.geRenInfo[25].value, //签订合同类别
          qdhtcs: this.geRenInfo[26].value, //签订合同次数
          sfxz: this.geRenInfo[27].value, //身份性质
          zlfs: this.geRenInfo[28].value, //招录方式
          zpr: this.geRenInfo[29].value, //招聘人
          fjlb: this.geRenInfo[30].value, //辅警类别
          yhkh: this.geRenInfo[31].value, //银行卡号
          khh: this.geRenInfo[32].value, //开户行
          lxdh: this.geRenInfo[33].value, //联系电话
          hjdz: this.geRenInfo[34].value, //户籍地址
          czdz: this.geRenInfo[35].value, //常住地址
          txdz: this.imageUrl //头像地址
        }
        postAction('/rsda/fjJbxx/add', obj)
          .then(res => {
            if (res.success == true) {
              that.gerenid = res.result.id
              that.$message.success(res.message)
            } else {
              that.$message.error(res.message)
            }
          })
          .finally(() => {
            that.confirmLoading = false
          })
      } else {
        let obj = {
          id: this.gerenid,
          xm: this.geRenInfo[0].value, //姓名
          xb: this.geRenInfo[1].value, //性别
          mz: this.geRenInfo[2].value, //民族
          sfzh: this.geRenInfo[3].value, //身份证号码
          kb: this.geRenInfo[4].value, //库别
          jsrq:this.geRenInfo[5].value,//减少时间
          dacsny: this.geRenInfo[6].value, //档案出生年月
          sfzcsny: this.geRenInfo[7].value, //身份证出生年月
          jb: this.geRenInfo[8].value, //级别
          fjbh: this.geRenInfo[9].value, //辅警编号
          ssdw: this.geRenInfo[10].value, //所属单位
          xl: this.geRenInfo[11].value, //学历
          xw: this.geRenInfo[12].value, //学位
          zzmm: this.geRenInfo[13].value, //政治面貌
          rdsj: this.geRenInfo[14].value, //入党时间
          szdzb: this.geRenInfo[15].value, //所在党支部
          sffy: this.geRenInfo[16].value, //是否服役
          twsj: this.geRenInfo[17].value, //退伍时间
          byyx: this.geRenInfo[18].value, //毕业院校
          sxzy: this.geRenInfo[19].value, //所学专业
          cjgagzsj: this.geRenInfo[20].value, //参加公安工作时间
          cjgzsj: this.geRenInfo[21].value, //参加工作时间
          jydjzhm: this.geRenInfo[22].value, //就业登记证号码
          qdhtrq: this.geRenInfo[23].value, //签订合同日期
          htjzrq: this.geRenInfo[24].value, //合同截止日期
          qdhtlb: this.geRenInfo[25].value, //签订合同类别
          qdhtcs: this.geRenInfo[26].value, //签订合同次数
          sfxz: this.geRenInfo[27].value, //身份性质
          zlfs: this.geRenInfo[28].value, //招录方式
          zpr: this.geRenInfo[29].value, //招聘人
          fjlb: this.geRenInfo[30].value, //辅警类别
          yhkh: this.geRenInfo[31].value, //银行卡号
          khh: this.geRenInfo[32].value, //开户行
          lxdh: this.geRenInfo[33].value, //联系电话
          hjdz: this.geRenInfo[34].value, //户籍地址
          czdz: this.geRenInfo[35].value, //常住地址
          txdz: this.imageUrl //头像地址
        }
        putAction('/rsda/fjJbxx/edit', obj)
          .then(res => {
            if (res.success == true) {
              that.$message.success(res.message)
            } else {
              that.$message.error(res.message)
            }
          })
          .finally(() => {
            that.confirmLoading = false
          })
      }
    },
    validateSfzh(value) {
      if (this.IdentityCodeValid(value)) {
      } else {
        this.$message.warning('请输入正确的身份证号!')
        return
      }
    },
    IdentityCodeValid(code) {
      var city = {
        11: '北京',
        12: '天津',
        13: '河北',
        14: '山西',
        15: '内蒙古',
        21: '辽宁',
        22: '吉林',
        23: '黑龙江 ',
        31: '上海',
        32: '江苏',
        33: '浙江',
        34: '安徽',
        35: '福建',
        36: '江西',
        37: '山东',
        41: '河南',
        42: '湖北 ',
        43: '湖南',
        44: '广东',
        45: '广西',
        46: '海南',
        50: '重庆',
        51: '四川',
        52: '贵州',
        53: '云南',
        54: '西藏 ',
        61: '陕西',
        62: '甘肃',
        63: '青海',
        64: '宁夏',
        65: '新疆',
        71: '台湾',
        81: '香港',
        82: '澳门',
        91: '国外 '
      }
      var tip = ''
      var pass = true

      if (!code || !/^\d{6}(18|19|20)?\d{2}(0[1-9]|1[012])(0[1-9]|[12]\d|3[01])\d{3}(\d|X)$/i.test(code)) {
        //console.log("身份证号格式错误")
        tip = '身份证号格式错误'
        pass = false
      } else if (!city[code.substr(0, 2)]) {
        tip = '地址编码错误'
        pass = false
      } else {
        //console.log("地址编码错误")
        //18位身份证需要验证最后一位校验位
        if (code.length == 18) {
          //console.log("18位身份证需要验证最后一位校验位")
          code = code.split('')
          //∑(ai×Wi)(mod 11)
          //加权因子
          var factor = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2]
          //校验位
          var parity = [1, 0, 'X', 9, 8, 7, 6, 5, 4, 3, 2]
          var sum = 0
          var ai = 0
          var wi = 0
          for (var i = 0; i < 17; i++) {
            ai = code[i]
            wi = factor[i]
            sum += ai * wi
          }
          var last = parity[sum % 11]
          if (parity[sum % 11] != code[17]) {
            tip = '校验位错误'
            pass = false
          }
        }
      }
      //console.log("2121212")
      //if(!pass) alert(tip);
      return pass
    },
    handleCancel() {
      this.close()
    }
  }
}
</script>

<style lang="less" scoped>
.geren-box {
  margin: 0 30px;
  padding: 16px 10px;
  display: flex;
  align-items: center;
  border-bottom: solid 1px #d3e0e7;
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
  color: #262626;
}
.geRen-a {
  display: flex;
}
.jiben {
  position: relative;
  box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.15);
  border-radius: 5px;
  min-height: 100px;
  margin-bottom: 24px;
  border: solid 1px #eef3f6;
  .jibena {
    position: absolute;
    left: 40px;
    top: -20px;
    background-color: #ffffff;
    height: 40px;
    display: flex;
    width: 140px;
    align-items: center;
    justify-content: center;
    color: #49a0ed;
    font-size: 18px;
    font-weight: 500;
  }
  .jibenb {
    width: 65%;
    margin: 0 30px;
    padding: 10px 30px;
    display: flex;
    flex-wrap: wrap;
    padding-bottom: 30px;
    // justify-content: center;
    .jibenb_a {
      display: flex;
      // width: 346px;
      margin-top: 20px;
      .jibenb_a_name {
        color: black;
        width: 200px;
        font-size: 12px;
        padding-right: 10px;
        text-align: right;
        display: flex;
        justify-content: flex-end;

        align-items: center;
      }
      .jibenb_a_select {
        font-size: 12px;
        color: black;
      }
      .datapicker {
        font-size: 12px;
        color: black;
      }
      input:not([type='range']) {
        color: black;
      }
      input {
        font-family: 'Chinese Quote', -apple-system, BlinkMacSystemFont, 'Segoe UI', 'PingFang SC', 'Hiragino Sans GB',
          'Microsoft YaHei', 'Helvetica Neue', Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji',
          'Segoe UI Symbol';
        font-variant: tabular-nums;
        box-sizing: border-box;
        margin: 0;
        padding: 0;
        list-style: none;
        position: relative;
        display: inline-block;
        padding: 4px 11px;
        height: 32px;
        font-size: 12px;
        line-height: 1.5;
        color: black;
        background-color: #fff;
        background-image: none;
        border: 1px solid #d9d9d9;
        border-radius: 4px;
        transition: all 0.3s;
      }
      input::-webkit-input-placeholder {
        color: #bfbfbf;
      }
      input:-moz-placeholder {
        color: #bfbfbf;
      }
      input:-ms-input-placeholder {
        color: #bfbfbf;
      }
    }
  }
}
.avatar-uploader > .ant-upload {
  width: 128px;
  height: 128px;
}
/deep/ .ant-upload-select-picture-card {
  width: 240px !important;
  height: 331px !important;
}
.ant-upload-select-picture-card i {
  font-size: 32px;
  color: #999;
}

.ant-upload-select-picture-card .ant-upload-text {
  margin-top: 8px;
  color: #666;
}
.shangchuan-box {
  padding: 30px;
}
</style>