<template>
  <div class="selectSystem">
    <div
      v-show="spinning"
      style="width:100%;height:100%;position: absolute;z-index: 100;background: #fff;opacity: 0.4;
    display: flex;
    justify-content: center;
    align-items: center;
    pointer-events: auto;"
    >
      <img style="width:60px;" src="../../assets/jiazai.gif" />
    </div>
    <!-- <a-spin :spinning="true" style="width:100%;height:100%;"> -->

    <div class="mask" v-show="tips"></div>
    <div class="topbar clearfix">
      <div class="system-time left">{{ time }}</div>
      <div class="logout right flax">
        <div @click="download" style="margin-right: 50px;cursor: pointer;">
          <img class="outicon" src="../../assets/imgs/xiazai.png" alt="下载" />
          移动端下载
        </div>
        <div @click="handleLogout" style="cursor: pointer;">
          <img class="outicon" src="../../assets/imgs/quit.png" alt="图片" />
          退出
        </div>
      </div>
    </div>
    <div class="mainbox">
      <ul class="menu">
        <li class="mainbox-item-3" v-for="(item, index) in menuList" :key="index">
          <div class="btn" @click="changeSpinning(item.sysName)">
            <div class="sys-icon">
              <img class="sys-icon-img" :src="item.mobileImg" :alt="item.mobileImg" />
              <div @mouseenter="showTipsList(index)" @mouseleave="hideTipsList(index)">
                <div class="message" v-if="item.nums !== 0">{{ item.nums }}</div>
                <ul
                  class="message-list"
                  v-if="item.nums !== 0"
                  @mouseenter="showTipsList(index)"
                  @mouseleave="hideTipsList(index)"
                  :style="{display:item.status?'block':'none'}"
                >
                  <li class="message-list-item">
                    <span class="index">1</span>
                    <div class="msg-cont">放假值班人员安排排放假值班人员安排放假值班人员安排</div>
                  </li>
                  <li class="message-list-item">
                    <span class="index">1</span>
                    <div class="msg-cont">放假值值班人员安排放假值班人员安排放假值班人员安排</div>
                  </li>
                  <li class="message-list-item">
                    <span class="index">1</span>
                    <div class="msg-cont">放假值班人员安排排放假值班人员安排放假值班人员安排</div>
                  </li>
                  <li class="message-list-item">
                    <span class="index">1</span>
                    <div class="msg-cont">放假值值班人员安排放假值班人员安排放假值班人员安排</div>
                  </li>
                  <li class="message-list-item">
                    <span class="index">1</span>
                    <div class="msg-cont">放假值班人员安排排放假值班人员安排放假值班人员安排</div>
                  </li>
                  <li class="message-list-item">
                    <span class="index">1</span>
                    <div class="msg-cont">放假值班人员安排排放假值班人员安排放假值班人员安排</div>
                  </li>
                  <li class="message-list-item">
                    <span class="index">1</span>
                    <div class="msg-cont">放假值班人员安排排放假值班人员安排放假值班人员安排</div>
                  </li>
                </ul>
              </div>
            </div>
            <div class="sys-text">{{ item.title }}</div>
          </div>
        </li>
      </ul>
    </div>
    <div v-show="visible" @click="handleOk" class="tanchu">
      <div style="text-align: center;">
        <img src="../../assets/erweima.png" alt="扫一扫下载移动端">
      </div>
    </div>
    <!-- </a-spin> -->
  </div>
</template>

<script>
import { timeFix, curDateTime } from '@/utils/util'
import { getAction } from '@/api/manage'
import Vue from 'vue'

// import HeaderNotice from './HeaderNotice'
// import UserPassword from './UserPassword'
import { mapActions, mapGetters } from 'vuex'
// import { mixinDevice } from '@/utils/mixin.js'
export default {
  name: 'selectSystem',
  data() {
    return {
      tips: false,
      spinning: false,
      baseurl: window._CONFIG['domianURL'],
      visible: false,
      closable: false,
      menuList: [
        {
          sysName: 'znyx',
          mobileImg: require('../../assets/imgs/target.png'),
          title: '智能营销',
          status: false,
          nums: 0
        },
        {
          sysName: 'ygxt',
          mobileImg: require('../../assets/imgs/credit-card.png'),
          title: '信贷运营',
          status: false,
          nums: 0
        },
        {
          sysName: 'dhgl',
          mobileImg: require('../../assets/imgs/folder.png'),
          title: '季检年检',
          status: false,
          nums: 0
        },
        {
          sysName: 'fkpt',
          mobileImg: require('../../assets/imgs/risk.png'),
          title: '信贷风控',
          status: false,
          nums: 0
        },
        {
          sysName: 'hgpt',
          mobileImg: require('../../assets/imgs/business.png'),
          title: '合规平台',
          status: false,
          nums: 0
        },
        {
          sysName: 'znjx',
          mobileImg: require('../../assets/imgs/marketing.png'),
          title: '智能绩效',
          status: false,
          nums: 0
        },
        {
          sysName: 'rlzy',
          mobileImg: require('../../assets/imgs/teamwork.png'),
          title: '人力资源管理',
          status: false,
          nums: 4
        },
        {
          sysName: '',
          mobileImg: require('../../assets/imgs/setting.png'),
          title: '系统设置',
          status: false,
          nums: 1
        }
      ],
      time: curDateTime()
    }
  },
  mounted() {
    var _this = this //声明一个变量指向vue实例this,保证作用域一致
    this.timer = setInterval(function() {
      // _this.time =timeFix()+"   "+ curDateTime(); //修改数据date
      _this.time = curDateTime() //修改数据date
    }, 1000)
  },
  beforeDestroy() {
    if (this.timer) {
      clearInterval(this.timer) //在vue实例销毁钱，清除我们的定时器
    }
  },
  created() {
    this.getlist()
  },
  methods: {
    ...mapActions(['Logout']),
    getlist() {
      this.spinning = true
      let obj = {
        token: JSON.parse(window.localStorage.getItem('pro__Access-Token')).value
      }
      getAction('/sys/permission/getUserShouyeByToken', obj)
        .then(res => {
          console.log(res)
          if (res.success == true) {
            console.log(res.result)
            for (let i = 0; i < res.result.length; i++) {
              res.result[i].mobileImg = this.baseurl + '/' + res.result[i].mobileImg
            }
            this.menuList = res.result
          } else {
            this.$notification.warn({
              message: '提示',
              description: res.message,
              duration: 3
            })
          }
        })
        .finally(() => {
          this.spinning = false
        })
    },
    changeSpinning(name) {
      this.spinning = true
      if (name || name == '') {
        //   if (name == 'znyx')
        // {
        //    window.open('http://218.3.204.20:8015/jeecg/loginController.do?login','target','');
        // }
        if (name == 'znjx') {
          this.spinning = false
          window.open('http://32.158.32.89:8080/cqpt/login', 'target2', '')
        } else {
          Vue.ls.set('SYS_NAME', name)
          Vue.ls.set('SYS_FLAG', true)
          // this.ls.set("SYS_NAME",name);
          this.$router.push({ name: 'dashboard' })
        }
      } else {
        this.$notification['error']({
          message: '提示',
          description: '缺少参数，无法跳转',
          duration: 3
        })
      }
    },
    jump(name) {
      var that = this
      that.spinning = true

      if (name || name == '') {
        //   if (name == 'znyx')
        // {
        //    window.open('http://218.3.204.20:8015/jeecg/loginController.do?login','target','');
        // }
        if (name == 'znjx') {
          window.open('http://32.158.32.89:8080/cqpt/login', 'target2', '')
        } else {
          Vue.ls.set('SYS_NAME', name)
          Vue.ls.set('SYS_FLAG', true)
          // this.ls.set("SYS_NAME",name);
          this.$router.push({ name: 'dashboard' })
        }
        that.spinning = false
      } else {
        this.$notification['error']({
          message: '提示',
          description: '缺少参数，无法跳转',
          duration: 3
        })
        that.spinning = false
      }
    },
    showTipsList(e) {
      //鼠标移入显示
      this.tips = true
      for (let i = 0; i < this.menuList.length; i++) {
        this.menuList[i].status = false
      }
      this.menuList[e].status = true
    },
    hideTipsList() {
      //鼠标移出显示
      this.tips = false
      for (let i = 0; i < this.menuList.length; i++) {
        this.menuList[i].status = false
      }
    },

    handleLogout() {
      const that = this
      this.$confirm({
        title: '提示',
        content: '真的要注销登录吗 ?',
        onOk() {
          return that
            .Logout({})
            .then(() => {
              window.location.href = '/'
              //window.location.reload()
            })
            .catch(err => {
              that.$message.error({
                title: '错误',
                description: err.message
              })
            })
        },
        onCancel() {}
      })
    },
    //下载移动端
    download() {
      this.visible = true;
    },
    handleOk(e) {
        console.log(e);
        this.visible = false;
      },
  }
}
</script>

<style lang="scss" scoped>
@import url('../../assets/css/reset.css');

ul {
  margin-bottom: 0;
}
.left {
  float: left;
}
.right {
  float: right;
}
.tanchu{
  top: 0px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1000;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.65);
}
.clearfix:after {
  content: '.';
  display: block;
  height: 0;
  visibility: hidden;
  clear: both;
}
.clearfix {
  *zoom: 1;
}
.flax {
  display: flex;
}
.selectSystem {
  width: 100%;
  min-height: 100%;
  background: url(../../assets/imgs/bg.png) no-repeat;
  background-size: 100% 100%;
  .mask {
    width: 100%;
    height: 100%;
    position: absolute;
    background-color: rgba($color: #000000, $alpha: 0.5);
    z-index: 10;
  }
  .topbar {
    font-size: 18px;
    width: 100%;
    height: 60px;
    line-height: 60px;
    color: #fff;
    background-color: #427cbb;
    -moz-box-shadow: 0px 7px 5px #040e37;
    -webkit-box-shadow: 0px 7px 5px #040e37;
    box-shadow: 0px 7px 5px #040e37;
    padding: 0 30px;
    margin-bottom: 5%;
    .system-time {
      margin-right: 35px;
    }
    .logout {
      .outicon {
        width: 20px;
        display: inline-block;
      }
    }
  }
  .menu {
    width: 100%;
    li.mainbox-item-3 {
      display: inline-block;
      padding-bottom: 30px;
      text-align: center;
      .btn {
        transition: all 0.5s;
        display: inline-block;
        cursor: pointer;
        .sys-icon {
          position: relative;
          display: inline-block;
          background-color: #fff;
          width: 178px;
          padding: 40px;
          text-align: center;
          border-radius: 5px;
          .sys-icon-img {
            width: 90px;
            height: 90px;
          }
          .message {
            position: absolute;
            z-index: 5;
            top: -15px;
            right: -15px;
            width: 30px;
            height: 30px;
            background-color: #ff4749;
            color: #fff;
            text-align: center;
            line-height: 30px;
            -moz-box-shadow: 0px 0px 5px #fff;
            -webkit-box-shadow: 0px 0px 5px #fff;
            box-shadow: 0px 0px 5px #fff;
            border-radius: 50%;
            z-index: 10;
          }
          .message-list {
            display: none;
            box-sizing: border-box;
            position: absolute;
            z-index: 3;
            top: 0;
            left: 178px;
            border-radius: 10px;
            background-color: #fff;
            width: 300px;
            min-height: 180px;
            max-height: 280px;
            overflow-y: auto;
            padding: 20px;
            color: #666;
            -moz-box-shadow: 0px 0px 5px #000;
            -webkit-box-shadow: 0px 0px 5px #000;
            box-shadow: 0px 0px 5px #000;
            z-index: 11;
            .message-list-item {
              position: relative;
              padding: 10px;
              font-size: 14px;
              padding-left: 30px;
              .index {
                position: absolute;
                border-radius: 50%;
                top: 10px;
                left: 0;
                width: 20px;
                height: 20px;
                text-align: center;
                line-height: 20px;
                background-color: #47a5fd;
                color: #fff;
              }
              .msg-cont {
                width: 100%;
                text-align: left;
                overflow: hidden;
                text-overflow: ellipsis;
                white-space: nowrap;
              }
            }
          }
          .message:hover + .message-list {
            display: block;
          }
        }
        .sys-text {
          transition: all 0.5s;
          line-height: 50px;
          color: #fff;
          font-size: 18px;
          font-weight: 200;
          text-align: center;
        }
      }
    }
  }
}
@media only screen and (max-width: 992px) {
  .sys-icon {
    width: 148px;
  }
}
.btn:hover > .sys-text {
  -moz-box-shadow: 0px 5px 10px #fff;
  -webkit-box-shadow: 0px 5px 10px #fff;
  box-shadow: 0px 5px 10px #fff;
}
</style>
