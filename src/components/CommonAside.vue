<template>
  <el-menu default-active="1-4-1" class="el-menu-vertical-demo" background-color="#545c64" 
  active-text-color="#ffd04b" text-color="#fff" @open="handleOpen" @close="handleClose" :collapse="isCollapse">
    <h3>{{isCollapse? "后台" : "后台管理系统"}}</h3>

    <el-menu-item @click="clickMenu(item)" v-for="item in noChildren" :index="item.path + ''" :key="item.path">
      <i :class="'el-icon-' + item.icon"></i>
      <span slot="title">{{item.label}}</span>
    </el-menu-item>
    
    <!-- index为遍历字符串，如果是数字，必须加个空字符串 -->
    <el-submenu v-for="item in hasChildren" :index="item.path+''" :key="item.label">
      <template slot="title">
        <i :class="'el-icon-' + item.icon"></i>
        <span slot="title">{{item.label}}</span>
      </template>

      <el-menu-item-group v-for="(subItem,subIndex) in item.children" :key="subItem.path">
        <el-menu-item @click="clickMenu(subItem)" :index="subIndex.toString()">{{subItem.label}}</el-menu-item>
      </el-menu-item-group>
    </el-submenu>

  </el-menu>
</template>

<style lang="less" scoped>
.el-menu-vertical-demo:not(.el-menu--collapse) {
    width: 200px;
    min-height: 400px;
}
.el-menu {   
    // height: 100vh;
    height: 100%;
    border: none;
    h3{
        color: #fff;
        text-align: center;
        line-height: 48px;
    }
}
</style>

<script>
export default {
  data() {
    return {
      menu: []
      };
  },
  methods: {
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    },
    clickMenu(item) {
        //因为router已经全局配置，所以这里可用
        this.$router.push({
            name: item.name
        })
        this.$store.commit('selectMenu',item)
    }
  },
  computed: {
    noChildren(){
        // return this.menu.filter(item => !item.children)
        return this.asyncMenu.filter(item => !item.children)
    },
    hasChildren(){
        // return this.menu.filter(item => item.children)
        return this.asyncMenu.filter(item => item.children)
    },
    isCollapse(){
        console.log(this)
        return this.$store.state.tab.isCollapse
    },
    asyncMenu(){
        return this.$store.state.tab.menu
    }
  }
};
</script>