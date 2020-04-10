<template>
  <div style="width:400px;margin-bottom:60px;background-color: #ffffff;padding-bottom:30px">
    <p class="biaotoi">扶贫贷款</p>
    <div style="width:360px;margin:0 auto">
       <canvas id="fpdk" width="360" height="220"></canvas>
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
     getAction('/business/fkxtJgjc/list', obj).then(res => {
        //console.log(res)
        if (res.success == true) {
          Meter5.setOptions({
              element: 'fpdk',
              centerPoint: {
                  x: 180,
                  y: 180
              },
              radius: 180,
              data: {
                  value:  res.result.fpdk,
                  title: '扶贫贷款{t}',
                  subTitle: '',
                  area: [{
                      min: 0, max: 20, text: ''
                  },{
                      min: 20, max: 40, text: ''
                  },{
                      min: 40, max: 60, text: ''
                  },{
                      min: 60, max: 80, text: ''
                  },{
                      min: 80, max: 100, text: ''
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
