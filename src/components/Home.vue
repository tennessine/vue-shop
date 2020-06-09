<template>
  <el-container class="home-container">
    <el-header>
      <div>
        <i
          class="toggle-button"
          :class="isCollapse ? 'el-icon-s-unfold': 'el-icon-s-fold'"
          @click="toggleCollapse"
        ></i>
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <el-container>
      <el-aside :width="isCollapse ? '64px': '200px'">
        <el-menu
          background-color="#545c64"
          text-color="#fff"
          active-text-color="#409eff"
          unique-opened
          :collapse="isCollapse"
          :collapse-transition="false"
          router
          :default-active="$router.currentRoute.path"
        >
          <el-submenu :index="`${item.id}`" v-for="item in menuList" :key="item.id">
            <template slot="title">
              <i class="el-icon-platform-eleme"></i>
              <span>{{item.authName}}</span>
            </template>
            <el-menu-item
              :index="`/${subItem.path}`"
              v-for="subItem in item.children"
              :key="subItem.id"
            >
              <i class="el-icon-menu"></i>
              <span slot="title">{{subItem.authName}}</span>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      isCollapse: false,
      menuList: []
    }
  },
  created() {
    this.getMenuList()
  },
  methods: {
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) {
        return this.$message.error(res.meta.message)
      }
      this.menuList = res.data
    },
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    }
  }
}
</script>

<style lang="scss" scoped>
.home-container {
  height: 100%;
}
.el-header {
  background: #373d41;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #fff;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    img {
      width: 30px;
      height: 30px;
    }
    span {
      margin-left: 15px;
    }
  }
}
.el-aside {
  background: #333744;
  .el-menu {
    border-right: none;
  }
}
.el-main {
  background: #eaedf1;
}
.toggle-button {
  cursor: pointer;
}
</style>
