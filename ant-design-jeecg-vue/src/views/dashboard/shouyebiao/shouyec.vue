<template>
  <div style="width:400px;margin-bottom:60px;background-color: #ffffff;padding-bottom:30px">
    <p class="biaotoi">不良贷款变动率</p>
    <div style="width:360px;margin:0 auto">
       <canvas id="bldkBdl" width="360" height="220"></canvas>
    </div>
   
  </div>
</template>

<script>
import { getAction } from '@/api/manage'
export default {
  methods: {

  },
  created(){
  },
  mounted() {
    let obj = {}
     getAction('/business/sy/list', obj).then(res => {
        //console.log(res)
        if (res.success == true) {
          Meter3.setOptions({
              element: 'bldkBdl',
              centerPoint: {
                  x: 180,
                  y: 180
              },
              radius: 180,
              data: {
                  value:  res.result.bldkBdl,
                  title: '不良贷款变动率{t}',
                  subTitle: '',
                  area: [{
                      min: 0, max: 20, text: '正常'
                  },{
                      min: 20, max: 50, text: '正常'
                  },{
                      min: 50, max: 80, text: '正常'
                  },{
                      min: 80, max: 100, text: '预警'
                  },{
                      min: 100, max: 400, text: '异常'
                  }]
              }
          }).init(); 
        } else {
          // 接口失败
        }
      })
    
  },
  beforeDestroy() {
      
  },
  data() {
    return {
      
    };
  },
};
</script>
<style scoped>
.biaotoi {
    height: 50px;
    padding-left: 30px;
    font-size: 14px;
    color: #2c3538;
    margin: 0px;
    line-height: 50px;
    background-color: #fafafa;
    margin-bottom: 70px;
  }
</style>
