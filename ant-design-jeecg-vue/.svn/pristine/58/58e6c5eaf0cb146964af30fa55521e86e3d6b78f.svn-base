<template>
  <div class="sxsqindex">
    <div class="sxsqindex_top">
      <div class="sxsqindex_top_left">
        <a-row class="cards" style="width:97%;padding:25px 0px;box-shadow:0px 4px 10px  rgba(0, 0, 0,0.1);background-color: #fff;">
          <a-col :span="11" class="center" style="margin-right: 20px;">
            <div class="card-items">
              <a-row type="flex" justify="space-between" align="middle">
                <a-col :span="16" @click="tiaozhuan" style="cursor:pointer;">
                  <p class="f16 cf">员工异常行为待陈述</p>
                  <div class="f18">
                    <span class="f32 mr_10">{{daichenshu}}</span>笔
                  </div>
                </a-col>

                <a-col :span="8" >
                  <img style="width:62px;" src="../../assets/biao_a.png" />
                </a-col>
              </a-row>
            </div>
          </a-col>
          <a-col :span="11" class="center" style="cursor:pointer;margin-right: 20px;">
            <div class="card-items bg2">
              <a-row type="flex" justify="space-between" align="middle">
                <a-col :span="16">
                  <p class="f16 cf">待审批</p>
                  <div class="f18">
                    <span class="f32 mr_10">{{daiShenPiCount}}</span>笔
                  </div>
                </a-col>
                <a-col :span="8">
                  <img style="width:62px;" src="../../assets/biao_b.png" />
                </a-col>
              </a-row>
            </div>
          </a-col>
        </a-row>
        <div class="mainlista" style="width:97%;box-shadow:0px 4px 10px  rgba(0, 0, 0,0.1);margin-top:30px;height:538px;background-color: #fff;">
          <p class="paiming" style="margin:0px;width:91%;margin:0 auto;"><img src="../../assets/shubao.png" style="margin-right:6px;" />员工违规积分提醒</p>
        <a-table size="small" :columns="columns" :dataSource="dataSource" style=" width:91%;margin:0 auto; padding:10px 20px;" >
          
        </a-table>
        </div>
      </div>
      <div class="sxsqindex_top_right">
         <div class="mainlista" style="width:100%;box-shadow:0px 4px 10px  rgba(0, 0, 0,0.1);background-color: #fff;">
          <p class="paiming" style="margin:0px"><img src="../../assets/shubao.png" style="margin-right:6px;" />建设中</p>
        <a-table size="small" :columns="columnsa" :dataSource="dataSourcea" style=" width:100%; padding:10px 20px;min-height:307px" >
          
        </a-table>
        </div>
        <div class="mainlista" style="width:100%;box-shadow:0px 4px 10px  rgba(0, 0, 0,0.1);background-color: #fff;">
          <p class="paiming" style="margin:0px;margin-top:20px;"><img src="../../assets/shubao.png" style="margin-right:6px;" />建设中</p>
        <a-table size="small" :columns="columnsa"  :dataSource="dataSourcea" style=" width:100%; padding:10px 20px;min-height:307px" >
          
        </a-table>
        </div>
      </div>
    </div>
    
    <div class="main" style="background-color: #fff;">
      <!-- <div class="mainlist">
        <div class="mainlista" style="width:43%">
          <p class="paiming">员工违规积分排名</p>
        <a-table size="small" :columns="columns" :dataSource="dataSource" style=" width:100%; padding:10px 20px;" >
          
        </a-table>
        </div>
        <div class="mainlista" style="width:27%">
          <p class="paiming">建设中</p>
        <a-table size="small" :columns="columnsa" :dataSource="dataSourcea" style=" width:100%; padding:10px 20px;" >
          
        </a-table>
        </div>
        <div class="mainlista" style="width:27%">
          <p class="paiming">建设中</p>
        <a-table size="small" :columns="columnsa"  :dataSource="dataSourcea" style=" width:100%; padding:10px 20px;" >
          
        </a-table>
        </div>
        
      </div> -->
      
      <img class="beijinga" src="../../assets/layoutbg.png" />
    </div>
  </div>
</template>

<script>
// import { JeecgListMixin } from '@/mixins/chaxu'
import { getAction } from '@/api/manage'

const columns = [
  {
    title: '序号',
   dataIndex: 'xh',
     className: 'xszjg',
    width:60,
    align:"center",
  },
  {
    title: '机构',
    className: 'xszjg',
    dataIndex: 'xszjg',
    align: 'center'
  },
  {
    title: '行员名称',
    className: 'wgryXm',
    dataIndex: 'wgryXm',
    align: 'center'
  },
  {
    title: '违规积分',
    dataIndex: 'wgjf',
    align: 'center',
    width: 100
  }
  
]
const columnsa = [
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
    title: '行员名称',
    className: 'wgryXm',
    dataIndex: 'wgryXm',
    align: 'center'
  },{
    title: '行员名称',
    className: 'wgryXm',
    dataIndex: 'wgryXm',
    align: 'center'
  },
  {
    title: '违规积分',
    dataIndex: 'wgjf',
    align: 'center',
    width: 100
  }
  
]

const dataSource = [
]
const dataSourcea = []
export default {
	name: 'sxsqindex',
  data() {
    return {
      columnsa,
      dataSourcea:[],
      dataSource,
      total:"",
      chaXunNum: '',
      daichenshu:"",
      daiShenPiCount:"",
	  columns,
	 
    }
  },
  computed: {
     
  },
  created(){
    // this.getcanshu();
    // //查询
    this.chaXun();
  },
  methods:{
    tiaozhuan(){
      this.$router.push({  //核心语句
        path:'/business/HGYcxwHzbList',   //跳转的路径
      })
    },
	  getcanshu(){
      this.loadData()
    },
    //查询
    chaXun(){
      let obj = {}
      getAction('/business/hgShouYe/title', obj).then(res => {
        console.log(res)
        if (res.success == true) {
          //console.log('接口成功')
          this.daiShenPiCount = res.result.daiShenPiCount
          this.daichenshu = res.result.daichenshu    
          this.dataSource = res.result.wgjf
        }
      })
    }
  }
}
</script>

<style scoped lang="scss">
.sxsqindex_top{
  display: flex;
}
.sxsqindex_top_left{
  width: 55%;
}
.sxsqindex_top_right{
  width: 45%;
}
@import url('../../assets/css/reset.css');
.center {
  text-align: center;
}
.paiming{
  font-size: 16px;
  color: #000000;
  margin-top: 15px;
  padding: 0px 20px;
  padding-top: 20px;
  color: #e35c5c;
  font-size: 14px;
  display: flex;
  align-items: center;
}
.mainlist{
  display: flex;
  // flex-wrap: wrap;
  justify-content: space-between;
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
  background-color: #fff;ff
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
  width: 100%;
  margin: 0 auto;
  // padding:0 30px;
  // padding-bottom: 20px;
  margin-bottom: 40px;
  // border: 1px solid #d8e2f0;
  border-radius: 10px;
  margin-top: 30px;
  padding: 30px 0px;
  text-align: center;
  .tco {
    color: #e35c5c;
  }
}
.beijinga {
  width: 90%;
  margin-bottom: -4px;
  border-radius: 10px;
}
</style>