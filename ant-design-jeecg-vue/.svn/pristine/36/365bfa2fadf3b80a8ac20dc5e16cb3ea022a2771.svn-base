<template>
  <div class="user-wrapper" :class="theme">
    <header-notice class="action"/>
    <a-dropdown>
      <span class="action action-full ant-dropdown-link user-dropdown-menu">
        <a-avatar class="avatar" size="small" :src="getAvatar()"/>
        <span v-if="isDesktop()">欢迎您，{{ nickname() }}</span>
      </span>
      <a-menu slot="overlay" class="user-dropdown-menu-wrapper">
        <a-menu-item key="1" @click="handleEdit">
          <a-icon type="user"/>
          <span>账户设置</span>
        </a-menu-item>
        <a-menu-item key="2" @click="updatePassword">
          <a-icon type="setting"/>
          <span>密码修改</span>
        </a-menu-item>
       <!-- <a-menu-item key="2" disabled>
          <a-icon type="setting"/>
          <span>测试</span>
        </a-menu-item>
        <a-menu-divider/>
        <a-menu-item key="3">
          <a href="javascript:;" @click="handleLogout">
            <a-icon type="logout"/>
            <span>退出登录</span>
          </a>
        </a-menu-item>-->
      </a-menu>
    </a-dropdown>
    <span class="action">
      <a class="logout_title" href="javascript:;" @click="handleLogout">
        <a-icon type="logout"/>
        <span v-if="isDesktop()">&nbsp;注销登录</span>
        <!-- <span v-if="isDesktop()">&nbsp;退出登录</span> -->
      </a>
    </span>
    <user-password ref="userPassword"></user-password>
    <user-modal ref="modalForm" @ok="modalFormOk"></user-modal>
  </div>
</template>

<script>
  import HeaderNotice from './HeaderNotice'
  import UserPassword from './UserPassword'
  import UserModal from '../../views/system/modules/UserModalEdit'
  import { mapActions, mapGetters } from 'vuex'
  import { getAction } from '@/api/manage'
  import { mixinDevice } from '@/utils/mixin.js'
  import { USER_INFO} from "@/store/mutation-types"
  import Vue from 'vue'

  export default {
    name: "UserMenu",
    mixins: [mixinDevice],
    components: {
      HeaderNotice,
      UserPassword,
      UserModal
    },
    props: {
      theme: {
        type: String,
        required: false,
        default: 'dark'
      }
    },
    methods: {
      ...mapActions(["Logout"]),
      ...mapGetters(["nickname", "avatar","userInfo"]),
      getAvatar(){
        //console.log('url = '+ window._CONFIG['imgDomainURL']+"/"+this.avatar())
        return window._CONFIG['imgDomainURL']+"/"+this.avatar()
      },
      handleLogout() {
        const that = this
				// this.$router.push({ name: "selectSystem" })
        // window.location.reload()

        this.$confirm({
          title: '提示',
          content: '真的要注销登录吗 ?',
          onOk() {
            return that.Logout({}).then(() => {
                window.location.href="/";
              //window.location.reload()
            }).catch(err => {
              that.$message.error({
                title: '错误',
                description: err.message
              })
            })
          },
          onCancel() {
          },
        });
      },
      updatePassword(){
        let username = this.userInfo().username
        this.$refs.userPassword.show(username)
      },
      handleEdit: function () {

        getAction('/sys/user/queryById',{id:this.userInfo().id}).then((res)=>{
          if(res.success){
            this.$refs.modalForm.edit(res.result);
            this.$refs.modalForm.title = "编辑";
          }
          else
          {
            that.$message.warning(res.message);
          }
        })
      },
      modalFormOk() {
        getAction('/sys/user/queryById',{id:this.userInfo().id}).then((res)=>{
          if(res.success){
            Vue.ls.set(USER_INFO, res.result, 7 * 24 * 60 * 60 * 1000);
          }
        })
      }
    }
  }
</script>

<style scoped>
  .logout_title {
    color: inherit;
    text-decoration: none;
  }
</style>