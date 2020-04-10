<template>
  <div style="width:400px;margin-bottom:60px;background-color: #ffffff;padding-bottom:30px">
    <p class="biaotoi">存贷比率</p>
    <div style="width:360px;margin:0 auto">
       <canvas id="cdbl" width="360" height="220"></canvas>
    </div>
  </div>
</template>

<script>
import { getAction } from '@/api/manage'
export default {
  created(){
     //console.log( this.cdbl )
  },
  methods:{

  },
  mounted() {
    //console.log( this.cdbl )
    let obj = {}
     getAction('/business/sy/list', obj).then(res => {
        //console.log(res)
        if (res.success == true) {
          Meter.setOptions({
              element: 'cdbl',
              centerPoint: {
                  x: 180,
                  y: 180
              },
              radius: 180,
              data: {
                  value:  res.result.cdbl,
                  title: '存贷比率{t}',
                  subTitle: '',
                  area: [{
                      min: 0, max: 15, text: '正常'
                  },{
                      min: 15, max: 30, text: '正常'
                  },{
                      min: 30, max: 45, text: '正常'
                  },{
                      min: 45, max: 75, text: '预警'
                  },{
                      min: 75, max: 200, text: '异常'
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
  props: ['cdbl'],
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
