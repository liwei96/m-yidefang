<template>
  <header id="hh">
    <img class="back" src="~/assets/goback.png" alt @click="back" />
    <img src="~/assets/back-cls.png" alt="" class="cls" v-if="ispk" @click="gocontent">
    <img class="logo" src="~/assets/logo.png" alt />
    <div class="zixuns" @click="gotalk" v-if="kk">
      <img src="~/assets/header-talk.png" alt />
      <p v-if="totalnum > 0">{{ totalnum }}</p>
    </div>
    <img src="~/assets/mapcai.png" alt class="list" @click="btns" ref="showPanels"/>
    <ul class="cailist" v-show="list" ref="showPanel">
      <li class="cmn">
        <router-link :to="'/' + jkl">
          <span></span>
          <img src="~/assets/barhome.png" />
          <p>首 页</p>
        </router-link>
      </li>
      <li>
        <router-link :to="'/' + jkl + '/search'">
          <img src="~/assets/barsearch.png" />
          <p>楼盘查询</p>
        </router-link>
      </li>
      <li>
        <router-link :to="'/' + jkl + '/join'">
          <img src="~/assets/tegother.png" />
          <p>平台合作</p>
        </router-link>
      </li>
      <li>
        <router-link :to="'/' + jkl + '/weike/before/56'">
          <img src="~/assets/barke.png" />
          <p>买房百科</p>
        </router-link>
      </li>
      <li>
        <router-link :to="'/' + jkl + '/infos/46'">
          <img src="~/assets/barxun.png" />
          <p>房产资讯</p>
        </router-link>
      </li>
    </ul>
  </header>
</template>
<script>
export default {
  props: {
    jkl: {
      type: String
    },
    totalnum: {
      type: Number
    },
    baidu: {
      type: String
    }
  },
  data() {
    return {
      list: false,
      kk: true,
      host: 0,
      ispk: false
    };
  },
  methods: {
    gocontent(){
      this.$router.push('/'+this.jkl+'/content/'+sessionStorage.getItem('proid'))
    },
    back() {
      // if(this.$route.path.indexOf('weike') !=-1 || this.$route.path.indexOf('infos') !=-1){
      //   this.$router.push('/'+this.jkl)
      // }else
      if (sessionStorage.getItem("ccid")) {
        window.location.href =
          "http://recruit.jy1980.com/content/"+sessionStorage.getItem("ccid")+"?type="+sessionStorage.getItem('hostnum')
      } else {
        if (this.$route.path.indexOf("order") != -1) {
          this.$router.push("/" + this.jkl + "/home");
        } else {
          this.$router.go(-1);
        }
      }
    },
    btns() {
      // if (this.list) {
      //   this.list = false;
      // } else {
      //   this.list = true;
      // }
    },
    gotalk() {
      // window.location.href =
      //   "http://www.jy1980.com:9191/hangzhou/talk?reconnect=" + this.url;
      let urlid = sessionStorage.getItem("proid");
      let id = sessionStorage.getItem(urlid);
      if (id) {
        sessionStorage.setItem("staffid", id);
        let n = parseInt(sessionStorage.getItem(id));
        let total = parseInt(sessionStorage.getItem("total"));
        total = total - n;
        if (total != 0) {
          sessionStorage.setItem("total", total);
        } else {
          sessionStorage.removeItem("total");
        }
        sessionStorage.removeItem(id);
      }
      this.$router.push("/" + this.jkl + "/talk/" + urlid);
    }
  },
  mounted() {
    console.log(this.$route.path)
    if (this.$route.path.indexOf('pk')!==-1) {
      this.ispk=true
    }
    document.addEventListener("click", e => {
      if (this.$refs.showPanel) {
        let isSelf = this.$refs.showPanel.contains(e.target);
        let isSelfs = this.$refs.showPanels.contains(e.target);
        if (!isSelf&&!isSelfs) {
          this.list = false;
        }else {
          if (!this.list){
            this.list = true
          }else{
            this.list = false
          }
        }
      }
    });
    this.host = this.$store.state.host;
    let url = window.location.href;
    if (url.indexOf("content") == -1) {
      this.kk = false;
    }
    let newurl = url.split("?")[0];
    let id = this.$route.params.id;
    let name = sessionStorage.getItem("buildname");
    newurl += `?proid=${id}&name=${name}`;
    newurl = encodeURIComponent(newurl);
    this.url = newurl;
    url = url.split("?")[1];
    if (url && url.indexOf("token") != -1) {
      localStorage.setItem("wstoken", url.split("=")[1]);
    }
  }
};
</script>
<style lang="less" scoped>
header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  height: 2.75rem;
  z-index: 10;
  background-color: #fff;
  position: fixed;
  top: 0;
  z-index: 1000;
  border-bottom: 0.03125rem solid #f7f7f7;
  .back {
    width: 1.5rem;
    margin-left: 1rem;
  }
  .logo {
    width: 4.375rem;
  }
  .cls {
    width: 1.125rem;
    position: absolute;
    left: 3.6875rem;
  }
  .home {
    width: 1.25rem;
    margin-right: 1rem;
  }
  .zixuns {
    margin-right: 1.25rem;
    position: absolute;
    right: 9%;
    top: 0.68rem;
    img {
      width: 1.5rem;
    }
    p {
      position: absolute;
      width: 0.8125rem;
      height: 0.8125rem;
      border-radius: 50%;
      text-align: center;
      line-height: 0.8125rem;
      background-color: #ff4040;
      color: #fff;
      font-size: 0.625rem;
      top: -0.40625rem;
      right: -0.1875rem;
    }
  }
  .list {
    width: 1.5rem;
    margin-right: 4%;
  }
  .cailist {
    width: 9.375rem;
    background: rgba(41, 41, 41, 0.9);
    position: absolute;
    top: 2.5rem;
    border-radius: 0.375rem;
    z-index: 20000;
    right: 4%;
    li {
      position: relative;
      color: #e6e6e6;
      font-size: 0.9375rem;
      line-height: 3.125rem;
      a {
        width: 100%;
        display: flex;
        align-items: center;
      }
      p {
        border-bottom: 0.5px solid #545454;
        flex: 1;
        color: #e6e6e6;
      }
      img {
        width: 1.5rem;
        margin: 0;
        margin-left: 1.625rem;
        margin-right: 0.875rem;
        height: 1.5rem;
      }
    }
    .cmn {
      span {
        display: block;
        border: 0.4375rem solid transparent;
        border-bottom-color: rgba(41, 41, 41, 0.9);
        position: absolute;
        top: -0.875rem;
        right: 0.625rem;
      }
    }
  }
}
</style>
