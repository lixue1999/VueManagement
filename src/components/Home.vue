<template>
<el-container class="home-container">
  <!--头部区域-->
  <el-header>
    <div>
      <div class="avatar_box">
          <img src="../assets/logo.png" alt="">
      </div>
      <span>职工信息管理系统</span>
    </div>
    欢迎您: {{username}}
    <el-button type="info" @click="logout">退出</el-button>
  </el-header>
  <!--页面主体区域-->
  <el-container>
    <!--侧边栏-->
    <el-aside :width="isCollapse ? '64px' : '200px'">
      <div class="togglo-button" @click="toggleCollapse">|||</div>
      <el-menu :router="true" background-color="#333744" text-color="#fff"
      active-text-color="#409EFF" unique-opened :collapse="isCollapse" :collapse-transition="false"
      :default-active="activePath">
        <template v-for="item in this.menulist">
          <!-- 有子菜单的一级菜单 -->
          <el-submenu v-if="item.childMenus && item.childMenus.length > 0" :key="item.id" :index="item.url">
            <template slot="title">
              <i class="el-icon-menu"></i>
              <span>{{item.name}}</span>
            </template>
            <!--二级菜单-->
            <el-menu-item :index="'/' + lastItem.url" v-for="lastItem in item.childMenus" :key="lastItem.id" @click="saveNavState('/' + lastItem.url)">
              <template slot="title">
                <i class="el-icon-location"></i>
                <span>{{lastItem.name}}</span>
              </template>
            </el-menu-item>      
          </el-submenu>
          <!-- 没有子菜单的一级菜单 -->
          <el-menu-item v-else :index="'/'+item.url" :key="item.id" @click="saveNavState('/' + item.url)">
            <i class="el-icon-menu2"></i>
            <span>{{item.name}}</span> 
          </el-menu-item>
        </template>
      </el-menu>
    </el-aside>
    <!--右侧内容主体-->
    <el-main>
      <!--路由占位符-->
      <router-view></router-view>
    </el-main>
  </el-container>
</el-container>
</template>

<script>
export default {
  data () {
    return {
      menulist: [],
      isCollapse: false,
      activePath: ''
    }
  },
  created () {
    this.getMenuList(),
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList () {
      //get(后端地址)
      const { data: res } = await this.$axios.get('http://localhost:8080/Demo/menu/getAllMenu') // eslint-disable-line no-unused-vars
      // if (res.meta.staus !== 200) return this.$message.error(res.meta.msg)
      console.log(res)
      this.menulist = res.data
    },
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    //保存连接的激活状态
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath',activePath)
      this.activePath=activePath
    }
  },
  computed: {
    username () {
      return this.$store.getters.getUserName
    }
  }
}
</script>
<style lang='less' scoped>
.home-container{
  height: 100%;
}
.el-header {
  background-color: #373d41 ;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: wheat;
  font-size: 20px;
  > div {
    display: flex;
    align-items:center;
    span {
      margin-left: 50px;
    }
  }
}
.el-aside{
  background-color: #333744 ;
  .el-menu{
    border-right: none;
  }
}
.el-main{
  background-color: #EAEDF1;
}
.iconfont {
  margin-right: 10px;
}
.togglo-button{
  background-color: #4A5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
.avatar_box{
        height: 40px;
        width: 40px;
        border:1px solid #eee;
        border-radius: 50%;
        padding: 3px;
        box-shadow: 0 0 3px #ddd;
        position: relative;
        left: 30px;
        background-color: #fff;
        img{
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background-color: #eee;
        }
}
</style>
