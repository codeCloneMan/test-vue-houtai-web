<template>
  <header>
  <div class="l-content">
      <el-button plain icon="el-icon-menu" size="mini" @click="handleMenu"></el-button>
      <!-- <h3 style="color: #fff">首页</h3> //面包屑替代 -->
      <el-breadcrumb separator="/">
        <el-breadcrumb-item v-for="item in tags" :key="item.path" :to="{ path: item.path }">{{item.label}}</el-breadcrumb-item>
      </el-breadcrumb>
  </div>
  <div class="r-content">
      <el-dropdown trigger="click" size="mini">
        <span>
          <img class="user" :src="userImg" />
        </span>
        <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>个人中心</el-dropdown-item>
            <el-dropdown-item @click.native="logout">退出</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
  </div>
  </header>

</template>

<script>
import {mapState} from 'vuex'
export default {
  name: 'commonHeader',
  data() {
    return {
      userImg: require('../assets/images/user.png')
    }
  },
  methods: {
    handleMenu() {
      this.$store.commit('collapseMenu')
    },
    logout(){
        this.$store.commit('clearToken')
        this.$store.commit('clearMenu')
        this.$router.push('/login')
    }
  },
  computed: {
    ...mapState({
      tags: state => state.tab.tabsList
    })
  }
}
</script>

<style lang="less" scoped>
//重写面包屑的字体颜色
// .l-content /deep/ .el-breadcrumb__inner {
//   color: #3A6DF3 ;
// }
//或者这样写
// .myColor >>> .el-breadcrumb__inner {
//   color: #3A6DF3 ;
// }

header {
  display: flex;
  height: 100%;
  justify-content: space-between;
  align-items: center;
}
.l-content {
  display: flex;
  align-items: center;
  .el-button {
    margin-right: 20px;
  }
}
.r-content {
  .user {
    width: 40px;
    height: 40px;
    border-radius: 50%;
  }
}

</style>