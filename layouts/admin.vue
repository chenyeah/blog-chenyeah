<template>
    <div id="admin">
        <div class="topbar">
            <nuxt-link to="/blog" class="logo">
                <img src="~assets/img/admin/login_1.png" alt="">
            </nuxt-link>
            <div>
                <span @click="loginOut" class="loginOut">登 出</span>
            </div>
        </div>
        <div class="container">
            <div class="sidebar" :class="{'sidebar-mini':isminisidebar}">
                <li class="menu-top" @click="isminisidebar=!isminisidebar">
                    <i class="icon iconfont icon-category"></i>
                </li>
                <nuxt-link :to="n.path" class="menu-c" v-for="(n,i) in menuList" :key="i" @click="menuListActive=i" :class="[currentRoute==n.route?'active':'']">
                    <i class="icon iconfont" :class="[n.class_name,menuListActive==i?'active':'']"></i>
                    <span class="menu-name" v-show="!isminisidebar">{{n.name}}</span>
                </nuxt-link>
            </div>
            <div ref="adminContent" class="content">
                <nuxt/>
            </div>
        </div>

    </div>
</template>
<script>
import Cookie from "js-cookie";
export default {
  data() {
    return {
      menuList: [
        {
          class_name: "icon-homepage",
          name: "首页",
          route: "",
          path: "/admin"
        },
        {
          class_name: "icon-document",
          name: "文章列表",
          route: "article",
          path: "/admin/article"
        },
        {
          class_name: "icon-supply",
          name: "其他",
          route: "other",
          path: "/admin/other"
        },
        {
          class_name: "icon-upload",
          name: "存储桶",
          route: "upload",
          path: "/admin/upload"
        },
        {
          class_name: "icon-setup",
          name: "设置",
          route: "setting",
          path: "/admin/setting"
        }
      ],
      isminisidebar: false,
      menuListActive: ""
    };
  },
  middleware: "userAuth",
  computed: {
    currentRoute: function() {
      let a = this.$route.name.split("-")[1]
        ? this.$route.name.split("-")[1]
        : "";
      return a;
    }
  },
  methods: {
    loginOut() {
      Cookie.remove("token");
      this.$router.replace("/admin/signin");
    }
  },
  mounted() {
  }
};
</script>
<style lang="less" scoped>
#admin {
  .topbar {
    position: fixed;
    right: 0;
    left: 0;
    display: flex;
    height: 50px;
    background-color: #93b5cf;
    background-image: url("~assets/img/admin/texture.png");
    color: #fff;
    .logo {
      width: 50px;
      background: #fff;
      img {
        display: block;
        width: 50px;
        height: 50px;
      }
    }
    .logo + div {
      flex: 1;
      padding-right: 20px;
      text-align: right;
      line-height: 50px;
    }
  }
  .container {
    position: absolute;
    top: 50px;
    bottom: 0;
    overflow-y: auto;
    padding: 0;
    width: 100vw;
    .sidebar {
      position: fixed;
      overflow-y: auto;
      width: 200px;
      height: 100vh;
      background-color: #d4d6dc;
      color: #fff;
      text-align: center;
      transition: all 0.5s cubic-bezier(0.4, 0.01, 0.165, 0.99);
      @keyframes fadeIn {
        from {
          opacity: 0;
        }
        to {
          opacity: 1;
        }
      }
      @keyframes fadeOut {
        from {
          opacity: 1;
        }
        to {
          opacity: 0;
        }
      }
      &.sidebar-mini {
        width: 50px;
        transform: translate3d(0px, 0px, 0px);
        .menu-name {
          animation: fadeOut 0.5s cubic-bezier(0.4, 0.01, 0.165, 0.99);

          animation-fill-mode: forwards;
        }
      }
      &::-webkit-scrollbar {
        display: none;
      }
      li.menu-top {
        height: 30px;
        border-bottom: 0;
        background: #575f6f;
        list-style: none;
        line-height: 30px;
        &:hover {
          background: #575f6f;
        }
      }
      .menu-c {
        display: block;
        height: 60px;
        background: #aeb3be;
        color: #fff;
        list-style: none;
        text-decoration: none;
        line-height: 60px;
        cursor: pointer;
        .menu-name {
          display: inline-block;
          width: 100px;
          text-align: left;
          animation: fadeIn 1s cubic-bezier(0.4, 0.01, 0.165, 0.99);

          animation-fill-mode: forwards;
        }
        &.active {
          background: #626c83;
          span {
            color: #fff;
          }
        }

        &:hover {
          background: #888fa0;
          span {
            color: #fff;
          }
        }
        span {
          display: inline-block;
          margin-left: 10px;
          color: #fafafa;
        }
      }
      .menu-c {
        .icon {
          vertical-align: sub;
          font-size: 30px;
        }
      }
    }
    .content {
      padding-left: 200px;
      background: #fafafa;
    }
  }
}
</style>
<style lang='less'>
</style>

