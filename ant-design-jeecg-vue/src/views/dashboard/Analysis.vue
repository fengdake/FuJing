<template >
  <div class="bground">
    <shouyea :cdbl="cdbl" ></shouyea>
    <shoueb :bldkBl="bldkBl"></shoueb>
    <shouyec :bldkBdl="bldkBdl" ></shouyec>
    <shouyed :bldkBdl="bldkZzbl"></shouyed>
  </div>

</template>

<script>
import shoueb from './shouyebiao/shouyeb'
import shouyea from './shouyebiao/shouea'
import shouyec from './shouyebiao/shouyec'
import shouyed from './shouyebiao/shouyed'
import { getAction } from '@/api/manage'
export default {
  name: 'Analysis',
  components: {
    shoueb,
    shouyea,
    shouyec,
    shouyed
  },
  data() {
    return {
      cdbl:"",
      bldkBl:"",
      bldkBdl:"",
      bldkZzbl:""
    }
  },
  created() {
    let obj = {}
     getAction('/business/sy/list', obj).then(res => {
        //console.log(res)
        if (res.success == true) {
          this.cdbl = res.result.cdbl
          this.bldkBl = res.result.bldkBl
          this.bldkBdl = res.result.bldkBdl
          this.bldkZzbl = res.result.bldkZzbl
        } else {
          // 接口失败
        }
      })
  },
  methods: {
  }
}
</script>

<style  scoped>
  .bground {
    width: 880px;
    height: 100%;
    padding:20px;
    display: flex;
    justify-content: center;
    align-items: center;
    /* background-color: #ffffff; */
    flex-wrap: wrap;
    align-items: center;
    justify-content: space-between;
    margin: 0 auto;
  }
</style>