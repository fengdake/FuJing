<template>
  <div style="width:85%" class="quanbu">
    <jiegoua></jiegoua>
    <jiegoub></jiegoub>
    <jiegouc></jiegouc>
    <jiegoud></jiegoud>
    <jiegoue></jiegoue>
    <jiegouf></jiegouf>
  </div>
</template>

<script>
import jiegoua from './jgjc/jisgou'
import jiegoub from './jgjc/jisgoub'
import jiegouc from './jgjc/jisgouc'
import jiegoud from './jgjc/jisgoud'
import jiegoue from './jgjc/jisgoude'
import jiegouf from './jgjc/jisgouf'
export default {
  name:"jiegou",
  components: {
    jiegoua,
    jiegoub,
    jiegouc,
    jiegoud,
    jiegoue,
    jiegouf
  },
  data() {
    return {}
  },
  methods: {},
  mounted() {
    
  },
  beforeDestroy() {},
  created() {},
  updated() {}
}
</script>
<style scoped>
.quanbu {
  width: 800px;
  height: 100%;
  padding: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  margin: 0 auto;
}
.biaotoi {
  height: 50px;
  padding-left: 30px;
  font-size: 14px;
  color: #2c3538;
  margin: 0px;
  line-height: 50px;
  background-color: #fafafa;
}
</style>
