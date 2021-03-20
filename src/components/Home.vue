<template>
  <el-container class="home_container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="../assets/avator.jpg" alt="" />
        <span>电商管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主体 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle_button" @click="toggleCollapse">|||</div>
        <!-- 侧边栏菜单 -->
        <el-menu
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409eff"
          unique-opened
          :collapse="isCollapse"
          :collapse-transition="false"
          :router="true"
          :default-active="activePath"
        >
          <!-- 一级菜单 -->
          <el-submenu :index="item.id + ''" v-for="item in menulist" :key="item.id">
            <!-- 一级菜单的模板区 -->
            <template slot="title">
              <!-- 图标 -->
              <i :class="iconObj[item.id]"></i>
              <!-- 文本 -->
              <span>{{ item.authName }}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item :index="'/'+subItem.path + ''" v-for="subItem in item.children" :key="subItem.id" @click="saveNavStatus('/'+subItem.path + '')">
              <template slot="title">
                <!-- 图标 -->
                <i class="el-icon-menu"></i>
                <!-- 文本 -->
                <span>{{ subItem.authName }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 页面内容主体 -->
      <el-main>
        <!-- 路由占用符 -->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data () {
    return {
      // 左侧菜单数据
      menulist: [],
      iconObj: {
        125: 'el-icon-s-custom',
        103: 'el-icon-s-unfold',
        101: 'el-icon-s-goods',
        102: 'el-icon-s-order',
        145: 'el-icon-s-comment'
      },
      // 是否折叠
      isCollapse: false,
      // 被激活的链接地址
      activePath: ''
    }
  },
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取所有菜单
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.errro(res.meta.msg)
      this.menulist = res.data
    },
    // 点击按钮切换菜单折叠与展开
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    saveNavStatus (activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style Lang="less" scoped>
.home_container {
  height: 100%;
}

.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #fff;
  font-size: 20px;
}

.el-header > div {
  display: flex;
  align-items: center;
}

.el-header > div > span {
  margin-left: 15px;
}

.el-header > div > img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
}

.el-aside {
  background-color: #333744;
  transition: all .4s ease-in-out;
}

.el-aside .el-menu {
  border-right: none;
}

.el-main {
  background-color: #eaedf1;
}

.toggle_button{
  background-color: #4a5046;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2rem;
  cursor: pointer;
}
</style>
