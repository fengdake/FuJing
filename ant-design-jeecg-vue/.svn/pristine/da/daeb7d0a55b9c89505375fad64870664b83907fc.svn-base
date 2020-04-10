<template>
  <div class="sxsqindex">
    <a-row class="cards">
      <a-col :span="6" class="center">
        <div class="card-items">
          <a-row type="flex" justify="space-between" align="middle">
            <a-col :span="16">
              <p class="f16 cf">今日待审批</p>
              <div class="f18">
                <span class="f32 mr_10">{{total}}</span>笔
              </div>
            </a-col>
            <a-col :span="8">
              <img style="width:62px;" src="../../assets/biao_a.png" />
            </a-col>
          </a-row>
        </div>
      </a-col>
      <a-col :span="6" class="center">
        <div class="card-items bg2">
          <a-row type="flex" justify="space-between" align="middle">
            <a-col :span="16">
              <p class="f16 cf">个人累计审批</p>
              <div class="f18">
                <span class="f32 mr_10">{{chaXunNum.totalCount}}</span>笔
              </div>
            </a-col>
            <a-col :span="8">
              <img style="width:62px;" src="../../assets/biao_b.png" />
            </a-col>
          </a-row>
        </div>
      </a-col>
      <a-col :span="6" class="center">
        <div class="card-items bg3">
          <a-row type="flex" justify="space-between" align="middle">
            <a-col :span="16">
              <p class="f16 cf">通过笔数</p>
              <div class="f18">
                <span class="f32 mr_10">{{chaXunNum.agreedCount}}</span>笔
              </div>
            </a-col>
            <a-col :span="8">
              <img style="width:62px;" src="../../assets/biao_c.png" />
            </a-col>
          </a-row>
        </div>
      </a-col>
      <a-col :span="6" class="center">
        <div class="card-items bg4">
          <a-row type="flex" justify="space-between" align="middle">
            <a-col :span="16">
              <p class="f16 cf">未通过笔数</p>
              <div class="f18">
                <span class="f32 mr_10">{{chaXunNum.notAgreedCount}}</span>笔
              </div>
            </a-col>
            <a-col :span="8">
              <img style="width:62px;" src="../../assets/biao_d.png" />
            </a-col>
          </a-row>
        </div>
      </a-col>
    </a-row>
    <div class="main">
      <a-table :columns="columns" :dataSource="dataSource" style="padding:10px 30px;">
        <template slot="name" slot-scope="text">
          <a href="javascript:;">{{text}}</a>
        </template>
        <template slot="title">
          <!-- <a-divider orientation="left"> -->
          <a-icon type="pushpin" style="font-size: 20px;margin-right: 10px;" />
          <span class="tco">待办消息（{{total}}）</span>
          <!-- </a-divider> -->
        </template>
      </a-table>
      <img class="beijinga" src="../../assets/layoutbg.png" />
    </div>
  </div>
</template>

<script>
import { JeecgListMixin } from '@/mixins/chaxu'
import { getAction } from '@/api/manage'

const columns = [
  {
    title: '序号',
    dataIndex: 'instid',
    width: 100
  },
  {
    title: '支行名称',
    dataIndex: 'depart_name',
    scopedSlots: { customRender: 'depart_name' },
    align: 'center',
    width: 200
  },
  {
    title: '当前环节',
    className: 'column-money',
    dataIndex: 'dqhj',
    align: 'center'
  },
  {
    title: '状态',
    dataIndex: 'lczt',
    align: 'center',
    width: 200
  },
  {
    title: '发起时间',
    dataIndex: 'starttime',
    align: 'center',
    width: 200
  }
]

const dataSource = [
]
export default {
	name: 'sxsqindex',
	mixins: [JeecgListMixin],
  data() {
    return {
      dataSource,
      total:"",
      chaXunNum: '',
	  columns,
	  url: {
        list: '/process/getShouYeApprovalList',
        delete: '/business/sxsq/delete',
        deleteBatch: '/business/sxsq/deleteBatch',
        exportXlsUrl: 'business/sxsq/exportXls',
        importExcelUrl: 'business/sxsq/importExcel'
      }
    }
  },
  computed: {
    importExcelUrl: function() {
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`
      //console.log(dataSource)
      //console.log(total)
    }
  },
  created(){
    this.getcanshu();
    //查询
    this.chaXun();
  },
  methods:{
	  getcanshu(){
      this.loadData()
    },
    //查询
    chaXun(){
      let obj = {}
      getAction('/process/getProcessCount', obj).then(res => {
        //console.log(res)
        if (res.success == true) {
          //console.log('接口成功')
          this.chaXunNum = res.result
        }
      })
    }
  }
}
</script>

<style scoped lang="scss">
@import url('../../assets/css/reset.css');
.center {
  text-align: center;
}
.ant-table td {
  white-space: nowrap;
}
.f32 {
  font-size: 26px;
}
.iconbg {
  display: inline-block;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  text-align: center;
  line-height: 60px;
  background-color: #fff;
  .icont {
    width: 30px;
    height: 30px;
    vertical-align: middle;
    color: #ff9e4d;
  }
}
.sxsqindex {
  width: 100%;
  height: 100%;
  background-color: #fff;
  padding-top: 15px;
  padding-bottom: 5px;
  .cards {
    .card-items {
      display: inline-block;
      -moz-box-shadow: 0px 2px 5px #c4ceff;
      -webkit-box-shadow: 0px 2px 5px #c4ceff;
      box-shadow: 0px 2px 5px #c4ceff;
      border-radius: 10px;
      width: 260px;
      // height: 105px;
      background-color: #ff9e4d;
      color: #fff;
      padding: 10px 15px;
    }
    .card-items.bg2 {
      background-color: #47a6ff;
      .icont {
        color: #47a6ff;
      }
    }
    .card-items.bg3 {
      background-color: #54d289;
      .icont {
        color: #54d289;
      }
    }
    .card-items.bg4 {
      background-color: #e35b5b;
      .icont {
        color: #e35b5b;
      }
    }
  }
}
.main {
  width: 90%;
  margin: 0 auto;
  // padding:0 30px;
  // padding-bottom: 20px;
  margin-bottom: 40px;
  border: 1px solid #d8e2f0;
  border-radius: 10px;
  margin-top: 20px;
  .tco {
    color: #e35c5c;
  }
}
.beijinga {
  width: 100%;
  margin-bottom: -4px;
  border-radius: 10px;
}
</style>